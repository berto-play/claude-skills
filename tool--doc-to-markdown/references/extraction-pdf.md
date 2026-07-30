# PDF Extraction (Text + Scanned)

Methods for extracting text-layer PDFs and scanned PDFs that need OCR.

## PDF (Text-Based)

### Detection

Sample the first 3 pages for extractable text. If all return empty strings or
whitespace-only, route to the scanned PDF pipeline.

```python
import pdfplumber

def has_text_layer(pdf_path, sample_pages=3):
    with pdfplumber.open(pdf_path) as pdf:
        for i, page in enumerate(pdf.pages[:sample_pages]):
            text = page.extract_text()
            if text and text.strip():
                return True
    return False
```

### Extraction with pdfplumber (preferred)

```python
import pdfplumber

def extract_pdf_text(pdf_path):
    pages = []
    with pdfplumber.open(pdf_path) as pdf:
        for i, page in enumerate(pdf.pages):
            text = page.extract_text() or ""
            tables = page.extract_tables()
            pages.append({
                "page_num": i + 1,
                "text": text,
                "tables": tables
            })
    return pages
```

### Table extraction

pdfplumber's table detection works well for most structured tables. For complex
tables with merged cells or unusual layouts:

```python
def extract_tables_with_settings(pdf_path):
    table_settings = {
        "vertical_strategy": "lines",
        "horizontal_strategy": "lines",
        "snap_tolerance": 5,
    }
    with pdfplumber.open(pdf_path) as pdf:
        for page in pdf.pages:
            tables = page.extract_tables(table_settings)
            # Try text-based strategy if lines-based finds nothing
            if not tables:
                table_settings_text = {
                    "vertical_strategy": "text",
                    "horizontal_strategy": "text",
                }
                tables = page.extract_tables(table_settings_text)
    return tables
```

### Fallback with pypdf

Use when pdfplumber fails (e.g., certain encrypted or malformed PDFs):

```python
from pypdf import PdfReader

def extract_pdf_pypdf(pdf_path):
    reader = PdfReader(pdf_path)
    pages = []
    for i, page in enumerate(reader.pages):
        text = page.extract_text() or ""
        pages.append({"page_num": i + 1, "text": text, "tables": []})
    return pages
```

### Metadata extraction

```python
from pypdf import PdfReader

def get_pdf_metadata(pdf_path):
    reader = PdfReader(pdf_path)
    meta = reader.metadata or {}
    return {
        "title": getattr(meta, 'title', None),
        "author": getattr(meta, 'author', None),
        "subject": getattr(meta, 'subject', None),
        "creator": getattr(meta, 'creator', None),
        "page_count": len(reader.pages),
    }
```

---

## PDF (Scanned)

### OCR Pipeline

Requires: `tesseract-ocr`, `poppler-utils`, `pytesseract`, `pdf2image`

```python
from pdf2image import convert_from_path
import pytesseract

def ocr_pdf(pdf_path, dpi=300, lang='eng'):
    """
    Convert scanned PDF to text via OCR.
    Use dpi=300 for good quality/speed balance.
    For multilingual docs, pass lang='eng+ita+fra' etc.
    """
    images = convert_from_path(pdf_path, dpi=dpi)
    pages = []
    for i, image in enumerate(images):
        text = pytesseract.image_to_string(image, lang=lang)
        pages.append({
            "page_num": i + 1,
            "text": text,
            "tables": [],
            "ocr": True,
            "confidence": estimate_ocr_confidence(image, lang)
        })
    return pages

def estimate_ocr_confidence(image, lang='eng'):
    """Rough confidence estimate from tesseract's detailed output."""
    data = pytesseract.image_to_data(image, lang=lang, output_type=pytesseract.Output.DICT)
    confidences = [int(c) for c in data['conf'] if int(c) > 0]
    if not confidences:
        return 0
    return sum(confidences) / len(confidences)
```

### OCR quality warnings

Always include an OCR quality note in the output:

```markdown
<!-- [OCR NOTE]: This content was extracted via OCR (tesseract).
     Average confidence: [X]%. Characters in low-confidence regions
     may be incorrect. Verify critical values against the original. -->
```

For confidence below 70%, add a stronger warning:

```markdown
<!-- [OCR WARNING]: Low OCR confidence ([X]%). This extraction likely
     contains errors. Do not use numeric values or proper nouns from
     this section without manual verification. -->
```

---

