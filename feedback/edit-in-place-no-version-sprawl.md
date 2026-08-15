---
name: edit-in-place-no-version-sprawl
description: When iterating on a design asset (flyer, business card, logo, etc.) across corrections, overwrite/edit the working file instead of saving a new v2/v3/v4... each time
metadata:
  type: feedback
  modified: 2026-08-15
---

Emile flagged (2026-08-15), while cleaning up `duovert-print/flyer/`, that iterative design work (flyer, business card, any asset refined over several correction rounds) had piled up as `flyer-front-v2-preview.png` through `v12`, plus unlabeled previews — 16 leftover files for what should have been one final asset per side.

**Why:** every correction round during a design session currently produces a brand-new numbered file rather than replacing the previous draft, so the working folder fills with intermediate junk that has to be manually cleaned out afterward (as happened here). Emile wants a clean folder as the default state, not a periodic cleanup task.

**How to apply:** while iterating on a single asset within one creative session (e.g. "make the logo bigger," "change this color"), overwrite/edit the same working file in place rather than writing a new `-v2`/`-v3` copy each time. Only create a genuinely separate file when it's a distinct deliverable (e.g. front vs. back of a flyer, or a real fork worth keeping both versions of) — not for sequential corrections to the same design. If a `final/` or equivalent subfolder convention is in use for a project (see `duovert-print/flyer/final/`), that's still the right place for the finished output, but intermediate rounds shouldn't accumulate as separate numbered files leading up to it.

See also: [[personal/mac-file-organization]]
