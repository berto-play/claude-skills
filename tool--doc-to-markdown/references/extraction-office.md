# Office Format Extraction (DOCX, EPUB, XLSX, CSV)

Methods for extracting structured office documents and tabular data.

## DOCX

### With mammoth (preferred for clean conversion)

```python
import mammoth

def extract_docx_mammoth(docx_path):
    with open(docx_path, "rb") as f:
        result = mammoth.convert_to_markdown(f)
        return {
            "text": result.value,
            "warnings": [str(w) for w in result.messages],
        }
```

### With pandoc (for tracked changes and complex formatting)

```bash
# Basic conversion
pandoc document.docx -t markdown -o output.md

# With tracked changes visible
pandoc document.docx -t markdown --track-changes=all -o output.md

# Preserve footnotes
pandoc document.docx -t markdown+footnotes -o output.md
```

### Direct XML parsing (for maximum control)

```python
import zipfile
from lxml import etree

def extract_docx_xml(docx_path):
    with zipfile.ZipFile(docx_path, 'r') as z:
        xml_content = z.read('word/document.xml')
        tree = etree.fromstring(xml_content)
        # Define namespaces
        ns = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}
        paragraphs = []
        for p in tree.findall('.//w:p', ns):
            texts = [t.text for t in p.findall('.//w:t', ns) if t.text]
            paragraphs.append(''.join(texts))
        return paragraphs
```

---

## EPUB

```python
import ebooklib
from ebooklib import epub
from bs4 import BeautifulSoup

def extract_epub(epub_path):
    book = epub.read_epub(epub_path)
    chapters = []

    # Get the reading order from the spine
    spine_ids = [item_id for item_id, _ in book.spine]
    items_by_id = {item.get_id(): item for item in book.get_items()}

    for item_id in spine_ids:
        item = items_by_id.get(item_id)
        if item and item.get_type() == ebooklib.ITEM_DOCUMENT:
            soup = BeautifulSoup(item.get_content(), 'html.parser')
            title = soup.find(['h1', 'h2', 'h3'])
            chapters.append({
                "id": item.get_id(),
                "title": title.get_text() if title else f"Chapter {len(chapters) + 1}",
                "html": str(soup.body) if soup.body else str(soup),
                "text": soup.get_text(separator='\n'),
            })

    metadata = {
        "title": book.get_metadata('DC', 'title'),
        "author": book.get_metadata('DC', 'creator'),
        "language": book.get_metadata('DC', 'language'),
    }
    return {"chapters": chapters, "metadata": metadata}
```

---

## XLSX

```python
import pandas as pd

def extract_xlsx(xlsx_path):
    """Extract all sheets from an Excel file."""
    xls = pd.ExcelFile(xlsx_path)
    sheets = {}
    for sheet_name in xls.sheet_names:
        df = pd.read_excel(xls, sheet_name=sheet_name)
        sheets[sheet_name] = {
            "dataframe": df,
            "rows": len(df),
            "columns": list(df.columns),
            "markdown_table": df.to_markdown(index=False),
        }
    return sheets
```

### Large spreadsheets

For sheets with more than 100 rows, provide a summary first:

```python
def summarize_large_sheet(df, sheet_name):
    summary = f"## {sheet_name}\n\n"
    summary += f"**Rows:** {len(df)} | **Columns:** {len(df.columns)}\n\n"
    summary += "**Column names:** " + ", ".join(df.columns) + "\n\n"
    summary += "**First 10 rows:**\n\n"
    summary += df.head(10).to_markdown(index=False) + "\n\n"
    summary += "**Data types:**\n\n"
    for col in df.columns:
        summary += f"- `{col}`: {df[col].dtype}\n"
    return summary
```

---

## CSV

```python
import pandas as pd
import chardet

def extract_csv(csv_path):
    # Detect encoding
    with open(csv_path, 'rb') as f:
        raw = f.read(10000)
        detected = chardet.detect(raw)
        encoding = detected['encoding'] or 'utf-8'

    # Try common delimiters
    for delimiter in [',', ';', '\t', '|']:
        try:
            df = pd.read_csv(csv_path, encoding=encoding, delimiter=delimiter)
            if len(df.columns) > 1:
                return {
                    "dataframe": df,
                    "encoding": encoding,
                    "delimiter": delimiter,
                    "markdown_table": df.to_markdown(index=False),
                }
        except Exception:
            continue

    # Fallback: single column
    df = pd.read_csv(csv_path, encoding=encoding)
    return {
        "dataframe": df,
        "encoding": encoding,
        "delimiter": ",",
        "markdown_table": df.to_markdown(index=False),
    }
```

---

