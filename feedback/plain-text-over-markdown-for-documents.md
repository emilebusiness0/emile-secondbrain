---
name: plain-text-over-markdown-for-documents
description: For files Emile opens outside Obsidian, prose notes get plain .txt, structured/row-based lists get a real .xlsx — never raw markdown
metadata:
  type: feedback
---

Never hand Emile a `.md` file with markdown syntax (`#`, `|` tables, `**bold**`) for
anything he'll open directly in Finder — he's not viewing these in a markdown-aware app,
so it just shows raw symbols, "looks like code." Two corrections happened back to back
on the same document (2026-08-28, [[duo-vert/employee-hiring-plan|hiring candidates
list]]), so the real rule has two branches depending on content shape:

- **Structured / row-based data** (a list of people, a tracker, anything with
  columns/statuses he'll scan repeatedly) → go straight to a real spreadsheet, `.xlsx`
  built locally with openpyxl per the xlsx skill, same pattern as
  [[duo-vert/sheets-tracking|the Depenses sheet]]. Emile said explicitly he wants
  "Google Sheets, Docs, Word, something with actual formatting" — a bare `.txt` list was
  rejected as a second failed attempt right after the `.md` one. Don't route through
  plain text for this content shape at all.
- **Unstructured prose** (notes, a write-up, anything without columns) → plain `.txt`
  with clean spacing is still right.

**How to apply:** before building any new standalone file for Emile, ask "does this have
columns/rows a person would scan like a table?" — if yes, build `.xlsx` immediately, skip
markdown and skip plain text as intermediate guesses. If no, plain `.txt` is fine.
Exceptions either way: files already tied to an existing CSV/Sheets workflow (see
[[duo-vert/sheets-tracking]]), code, or anything explicitly destined for the Obsidian
vault where markdown renders correctly.
