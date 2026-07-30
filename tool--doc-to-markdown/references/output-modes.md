# Output Modes — Doc-to-MD

Three output modes. The user selects, or you ask them to choose. Each optimizes Markdown structure for a different downstream use.

---

## Mode 1: RAG / Vector DB

Optimized for chunking and semantic retrieval.

**Structure:**
- Split content into self-contained sections of 300-800 tokens each
- Each chunk gets a descriptive H2 heading that works as a standalone label
- Each chunk begins with a one-line context sentence: what document this is from, what section, and what the chunk covers
- Cross-references between chunks use a consistent ID scheme: `[See: chunk-id]`
- Tables are kept intact within a single chunk (never split across chunks)
- Metadata block at the top of the file

**Metadata block:**
```markdown
---
source_file: [original filename]
source_type: [PDF/DOCX/EPUB/etc.]
extraction_date: [ISO 8601]
total_chunks: [N]
language: [detected language]
confidence: [HIGH/MEDIUM/LOW based on source quality]
---
```

**Chunking rules:**
- Never split mid-sentence
- Never split a table, code block, or list
- Prefer splitting at heading boundaries, then paragraph boundaries
- Each chunk must be independently understandable (a reader seeing only that chunk should know what document it came from and what section they are reading)

---

## Mode 2: LLM Context Window

Optimized for pasting into a single LLM prompt.

**Structure:**
- Single continuous document with a clear heading hierarchy
- Metadata summary at the top (title, author, date, page count, type)
- Table of contents with section links
- Aggressive deduplication: if the source repeats content (headers, footers, boilerplate), include it once and note the repetition
- Preserve all substantive content but strip purely decorative elements
- Keep total output as compact as possible without losing information

**Format:**
```markdown
# [Document Title]

> **Source:** [filename] | **Type:** [format] | **Pages/Sections:** [count] | **Date:** [if available]

## Table of Contents
- [Section 1](#section-1)
- [Section 2](#section-2)
...

## Section 1
[content]
```

---

## Mode 3: Knowledge Base / Wiki

Optimized for Obsidian, Notion, or similar tools.

**Structure:**
- Rich heading hierarchy (H1 through H4)
- Wikilinks for cross-references: `[[Related Topic]]`
- Tags in YAML frontmatter
- Callouts for warnings, notes, and definitions
- Footnotes preserved and linked
- Each major section could stand alone as its own note

**Format:**
```markdown
---
title: [Document Title]
source: [filename]
type: [format]
tags: [auto-generated from content]
created: [extraction date]
---

# [Document Title]

> [!info] Source Document
> Extracted from `[filename]` on [date]. [quality notes if any].

## [Section]
[content]

> [!warning] Incomplete Section
> The source document appears to truncate here. Content may be missing.
```
