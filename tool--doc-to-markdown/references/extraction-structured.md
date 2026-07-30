# Structured Data Extraction (JSON, HTML, XML)

Methods for parsing structured data and markup formats into Markdown.

## JSON

```python
import json

def extract_json(json_path):
    with open(json_path, 'r', encoding='utf-8') as f:
        data = json.load(f)
    return {
        "data": data,
        "type": type(data).__name__,
        "structure": describe_json_structure(data),
    }

def describe_json_structure(data, depth=0, max_depth=4):
    """Describe JSON structure for the quality assessment."""
    indent = "  " * depth
    if depth > max_depth:
        return f"{indent}[nested beyond depth {max_depth}]"
    if isinstance(data, dict):
        lines = [f"{indent}object ({len(data)} keys):"]
        for key in list(data.keys())[:20]:
            lines.append(f"{indent}  {key}: {describe_json_structure(data[key], depth+1, max_depth)}")
        if len(data) > 20:
            lines.append(f"{indent}  ... and {len(data) - 20} more keys")
        return '\n'.join(lines)
    elif isinstance(data, list):
        if not data:
            return "empty array"
        return f"array ({len(data)} items) of {describe_json_structure(data[0], depth+1, max_depth)}"
    else:
        return type(data).__name__
```

### JSON to Markdown conversion rules

- **Object with string values:** key-value pairs as a definition list or table
- **Array of objects (uniform keys):** table with keys as column headers
- **Nested objects:** nested headings (H2 for top-level keys, H3 for nested)
- **Mixed/complex:** preserve as fenced JSON code block with annotations

---

## HTML

```python
from bs4 import BeautifulSoup
import re

def extract_html(html_path):
    with open(html_path, 'r', encoding='utf-8') as f:
        content = f.read()

    soup = BeautifulSoup(content, 'html.parser')

    # Remove script and style elements
    for tag in soup(['script', 'style', 'nav', 'footer', 'header']):
        tag.decompose()

    # Extract title
    title = soup.title.string if soup.title else None

    # Extract main content (prefer article/main tags)
    main = soup.find('main') or soup.find('article') or soup.body or soup

    return {
        "title": title,
        "text": main.get_text(separator='\n'),
        "html": str(main),
        "links": [(a.get_text(), a.get('href')) for a in main.find_all('a', href=True)],
    }
```

---

## XML

```python
from lxml import etree

def extract_xml(xml_path):
    tree = etree.parse(xml_path)
    root = tree.getroot()
    return {
        "root_tag": root.tag,
        "namespaces": dict(root.nsmap) if root.nsmap else {},
        "children": [child.tag for child in root],
        "text_content": etree.tostring(root, method="text", encoding="unicode"),
        "structure": describe_xml_structure(root),
    }

def describe_xml_structure(element, depth=0, max_depth=4):
    if depth > max_depth:
        return "..."
    tag = element.tag.split('}')[-1] if '}' in element.tag else element.tag
    children = list(element)
    if not children:
        return tag
    child_tags = [describe_xml_structure(c, depth+1, max_depth) for c in children[:10]]
    return f"{tag} -> [{', '.join(child_tags)}]"
```

---

