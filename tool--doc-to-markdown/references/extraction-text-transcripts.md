# Plain Text and Transcript Extraction (TXT, SRT, VTT)

Methods for plain text files and time-coded transcript formats.

## TXT

```python
import chardet

def extract_txt(txt_path):
    # Detect encoding
    with open(txt_path, 'rb') as f:
        raw = f.read()
        detected = chardet.detect(raw)
        encoding = detected['encoding'] or 'utf-8'

    with open(txt_path, 'r', encoding=encoding, errors='replace') as f:
        content = f.read()

    return {
        "text": content,
        "encoding": encoding,
        "line_count": content.count('\n') + 1,
        "char_count": len(content),
    }
```

---

## Transcripts

### SRT format

```python
import re

def extract_srt(srt_path):
    with open(srt_path, 'r', encoding='utf-8') as f:
        content = f.read()

    pattern = r'(\d+)\n(\d{2}:\d{2}:\d{2},\d{3}) --> (\d{2}:\d{2}:\d{2},\d{3})\n(.*?)(?=\n\n|\Z)'
    matches = re.findall(pattern, content, re.DOTALL)

    entries = []
    for idx, start, end, text in matches:
        entries.append({
            "index": int(idx),
            "start": start,
            "end": end,
            "text": text.strip().replace('\n', ' '),
        })
    return entries
```

### VTT format

```python
import re

def extract_vtt(vtt_path):
    with open(vtt_path, 'r', encoding='utf-8') as f:
        content = f.read()

    # Skip WEBVTT header
    content = re.sub(r'^WEBVTT.*?\n\n', '', content, flags=re.DOTALL)

    pattern = r'(?:(\d+)\n)?(\d{2}:\d{2}:\d{2}\.\d{3}) --> (\d{2}:\d{2}:\d{2}\.\d{3}).*?\n(.*?)(?=\n\n|\Z)'
    matches = re.findall(pattern, content, re.DOTALL)

    entries = []
    for idx, start, end, text in matches:
        # Strip VTT tags
        clean_text = re.sub(r'<[^>]+>', '', text).strip().replace('\n', ' ')
        entries.append({
            "start": start,
            "end": end,
            "text": clean_text,
        })
    return entries
```

### Transcript to Markdown

For all transcript formats, merge consecutive entries from the same speaker and
format as a dialogue:

```markdown
**[00:00:15] Speaker A:** First statement from speaker A that may span
multiple subtitle entries but belongs to one thought.

**[00:01:02] Speaker B:** Response from speaker B.
```

If no speaker labels are present, use timestamp-only format:

```markdown
**[00:00:15]** First passage of text.

**[00:01:02]** Second passage of text.
```

---

