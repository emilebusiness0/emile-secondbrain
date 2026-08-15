---
name: edit-in-place-no-version-sprawl
description: General preference — when revising any output (files, Google Sheets, docs, code, design assets, anything), edit/overwrite the existing version instead of creating a new copy each round
metadata:
  type: feedback
  modified: 2026-08-15
---

Emile flagged (2026-08-15), while cleaning up `duovert-print/flyer/` (which had piled up `flyer-front-v2-preview.png` through `v12`, 16 leftover files for what should have been one final asset), that this file had been saved as scoped to design assets specifically. **Corrected same day: this is a general preference, not limited to design files** — it applies to everything: local files, Google Sheets, documents, code, any deliverable that gets revised across multiple rounds.

**Why (two reasons, both stated directly):**
1. **Token cost** — recreating a file from scratch each revision round costs more tokens than editing the existing one in place.
2. **Clutter** — separate numbered copies (`v2`, `v3`, `v4`...) become trash files sitting around afterward, whether that's on disk, in a Google Sheet's version history/duplicate tabs, or anywhere else — Emile wants a clean end state by default, not something that needs a manual cleanup pass later (like the flyer folder did).

**How to apply:** default to editing/overwriting the existing version of whatever's being worked on — a file, a spreadsheet, a doc, a code file, a design asset — across correction rounds within the same task. Only create a genuinely separate copy when it's an actually distinct deliverable (e.g. front vs. back of a flyer, or a real fork worth keeping both versions of on purpose) — not as a byproduct of making a correction to the same thing. This applies regardless of tool or medium — not just local files Claude Code writes, but anything edited via any tool (Sheets, Docs, Drive, etc.).

See also: [[personal/mac-file-organization]]
