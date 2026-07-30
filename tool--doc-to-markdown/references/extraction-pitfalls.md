# Common Extraction Pitfalls

Cross-format gotchas to check before delivering output.

## Common Pitfalls

1. **Encoding issues:** Always detect encoding before reading. Never assume UTF-8.
   Use chardet for detection, and pass `errors='replace'` as a last resort.

2. **Memory with large files:** For PDFs over 200 pages or XLSX files over 50MB,
   process in batches. Never load the entire file into memory at once.

3. **Table misalignment:** pdfplumber sometimes merges or splits table cells
   incorrectly. Always verify table column counts against the source.

4. **PDF header/footer repetition:** Most PDFs repeat headers and footers on every
   page. Detect and deduplicate these in the LLM context window mode.

5. **EPUB reading order:** Always use the spine (reading order) rather than iterating
   all items, which may include navigation pages, cover images, and stylesheets.

6. **JSON arrays of primitives:** An array of strings or numbers should become a
   Markdown list, not a table.

7. **HTML boilerplate:** Always strip nav, footer, header, script, and style elements
   before extracting content.

8. **Transcript timestamps:** Different formats use different timestamp separators
   (comma in SRT, dot in VTT). Normalize to a consistent format in output.

9. **Mixed-content PDFs:** Some PDFs have both text layers and scanned pages. Check
   each page individually rather than assuming the whole file is one type.

10. **Password-protected files:** Detect and report early. Do not attempt brute force
    or guess passwords.
