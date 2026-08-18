---
name: mac-file-organization
description: Ongoing project to organize/clean up files on Emile's new Mac (got it ~2026-07-30)
metadata:
  type: project
  modified: 2026-08-15
---

Emile asked (2026-08-15) for help organizing and clearing out files on his Mac — got the computer about 2 weeks prior (see [[personal/about-emile]]), and a lot of one-time-use files have accumulated with no system in place yet.

**Why:** wants to start clean/organized now rather than let clutter compound, since this is a new machine.

**Cleanup executed 2026-08-15.** Turned out the Desktop "apps" weren't just misplaced — they were full duplicate installs (Claude 753M, Deezer 484M, Chrome 706M, Obsidian 514M = ~2.4GB) alongside already-working copies in `/Applications`, so they were trashed rather than moved. Built this structure under `~/Documents/` for anything that isn't a code repo:
- `Admin/` — DuoVert-Registration.PDF, `Cegep/` subfolder (originally two schedule PDFs, since superseded by a schedule photo — see [[personal/about-emile]] for the current source of truth)
- `Assets/Business-Card/`, `Assets/Flyer/`, `Assets/Logo-QR/`, `Assets/City-Photos/` — Duo Vert marketing assets that were stranded in Downloads
- `Assets/Website-Exports/` — the 10 home-*.jpg/png files that were loose directly in `~`, plus "image cover apple.jpg"

Trashed as genuine one-time junk (macOS Trash, recoverable): `Obsidian-1.13.4.dmg` installer, a stale `duovert-site-fixed-...zip` backup, plus the 4 duplicate apps above.

Deliberately left untouched: `duovert-site`, `duovert-print`, `duovert-infra` (live git repos, not clutter), and 2 screenshots on Desktop from 2026-08-15 (Emile hasn't said what to do with them yet).

**How to apply going forward:** this `Documents/Admin/` + `Documents/Assets/<category>/` pattern is now the standard — when new one-off files land in Downloads or the home root, sort them into this structure (or extend it with a new category subfolder) rather than reinventing an organization scheme each time. Home folder root and Downloads should stay empty/near-empty as a baseline; if they're accumulating again, that's a signal to redo a pass like this one.

See also: [[personal/about-emile]]
