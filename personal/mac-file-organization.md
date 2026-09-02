---
name: mac-file-organization
description: Ongoing project to organize/clean up files on Emile's new Mac (got it ~2026-07-30)
metadata:
  type: project
  modified: 2026-08-26
---

Emile asked (2026-08-15) for help organizing and clearing out files on his Mac — got the computer about 2 weeks prior (see [[personal/about-emile]]), and a lot of one-time-use files have accumulated with no system in place yet.

**Why:** wants to start clean/organized now rather than let clutter compound, since this is a new machine.

**Cleanup executed 2026-08-15.** Turned out the Desktop "apps" weren't just misplaced — they were full duplicate installs (Claude 753M, Deezer 484M, Chrome 706M, Obsidian 514M = ~2.4GB) alongside already-working copies in `/Applications`, so they were trashed rather than moved. Built this structure under `~/Documents/` for anything that isn't a code repo:
- `Admin/` — DuoVert-Registration.PDF, `Cegep/` subfolder (originally two schedule PDFs, since superseded by a schedule photo — see [[personal/about-emile]] for the current source of truth)
- `Assets/Business-Card/`, `Assets/Flyer/`, `Assets/Logo-QR/`, `Assets/City-Photos/` — Duo Vert marketing assets that were stranded in Downloads
- `Assets/Website-Exports/` — the 10 home-*.jpg/png files that were loose directly in `~`, plus "image cover apple.jpg"

Trashed as genuine one-time junk (macOS Trash, recoverable): `Obsidian-1.13.4.dmg` installer, a stale `duovert-site-fixed-...zip` backup, plus the 4 duplicate apps above.

Deliberately left untouched: `duovert-site`, `duovert-print`, `duovert-infra` (live git repos, not clutter), and 2 screenshots on Desktop from 2026-08-15 (Emile hasn't said what to do with them yet).

**How to apply going forward:** this `Documents/Duo Vert/Admin/` + `Documents/Duo Vert/Assets/<category>/` pattern is now the standard — when new one-off files land in Downloads or the home root, sort them into this structure (or extend it with a new category subfolder) rather than reinventing an organization scheme each time. Home folder root and Downloads should stay empty/near-empty as a baseline; if they're accumulating again, that's a signal to redo a pass like this one.

**Exception carved out 2026-08-18 — `soumissions/` lives as a sibling of `Admin/`, not nested under it.** The quote-generator script (see [[feedback/build-locally-not-live-browser]], [[duo-vert/soumission-template]]) originally wrote PDFs to `Admin/soumissions/`; Emile explicitly asked to move that folder up a level instead (now `Duo Vert/soumissions/` — see the 2026-08-26 restructure below). Don't nest client-facing deliverable folders (things he hands to clients, not internal admin paperwork) under `Admin/` — that subfolder is for internal reference docs (registration, contracts), not active outbound documents. If a similar "documents I send out" category comes up again, default to a folder that's a sibling of `Admin/`, not tucked inside it.

**Full restructure 2026-08-26 — everything business-related now nests under one `Documents/Duo Vert/` folder.** Emile asked for a strict "one folder per domain, subsections inside" model (using his Obsidian vault — one folder, subsections inside — as the reference pattern) rather than loose top-level folders in `Documents/`. Moved `Admin/`, `Assets/`, `soumissions/`, and the three live git repos (`duovert-site`, `duovert-print`, `duovert-infra`) from directly under `Documents/` into `Documents/Duo Vert/`. Added `Duo Vert/Finances/` for the two spreadsheets (`Duo Vert - Clients Revenu.xlsx`, `Duo Vert - Dépenses.xlsx`) that were previously loose in `Documents/`. Updated `duovert-quote-generator.py`'s hardcoded `OUTDIR` to the new `soumissions/` path, and every other vault file that referenced the old paths, in the same pass. Verified afterward: `git status` still works in all three repo locations, `node_modules` survived the `duovert-site` move intact. Checked first for anything outside the vault (shell rc files, crontab, launchd) hardcoding these paths — found none, so the repo move carried no external tooling risk.

Also discovered during this pass: `duovert-print` and `duovert-infra` are **not actually git repos** (`git status` returns "not a git repository") — only `duovert-site` has a working `.git`. The 2026-08-15 note above calling all three "live git repos" was wrong for two of them; corrected here.

The Cégep schedule image (`Admin/Cegep/Cegep Automne 2026.png`) moved out of the business folder entirely, into `Documents/Cegep/` (school content, not business) — see [[personal/cegep-school-organization]].

**Extended 2026-08-26** with a `Documents/Cegep/<Course Name>/` set for schoolwork — see [[personal/cegep-school-organization]] for the course list and sorting rules.

**Recurred and re-trashed 2026-08-26:** `Claude.app`, `Deezer.app`, `Obsidian.app` (~1.75GB) had reappeared directly under `Documents/` — same sizes as the ones trashed 2026-08-15, working copies confirmed still in `/Applications`. Emile confirmed trashing them again; cause of the recurrence is still unknown (not investigated) — if they show up a third time, worth checking whether something (an installer, a sync process) is recreating them.

**Root cause found 2026-08-26 — Desktop is a second, separate duplicate-accumulation spot, distinct from Documents.** Emile got confused when a Dock icon for Deezer said "in the trash" right after the Documents-copy cleanup. Investigation found: macOS Dock icons track the actual file (not just a saved path) — the Deezer Dock icon happened to be bookmarked to the `Documents/Deezer.app` copy, so when that copy got trashed, the Dock icon followed it into the Trash and broke. Separately, Finder showed "two" Deezer copies because `~/Desktop/` had its *own* full duplicate (484MB) that the 2026-08-15 cleanup never touched (it only checked Documents) — likewise for `Claude.app` (753MB) and `Obsidian.app` (514MB), all trashed 2026-08-26. Same day, a fresh Microsoft Office install left **Microsoft Word/Excel/PowerPoint duplicated on Desktop too, 7.4GB combined** (2.7+2.5+2.2GB) — flagged to Emile, confirmed and trashed. Desktop is now duplicate-free (~9.2GB reclaimed total across this pass).

**How to apply going forward:** when checking for duplicate/stray apps, check `~/Desktop/` in addition to `~/Documents/` and the home root — Desktop keeps independently accumulating full app duplicates (likely from how installers/downloads get opened — e.g. running an installer.pkg directly from Downloads seems to drop a full extracted copy on Desktop in addition to installing properly to `/Applications`). If a Dock icon breaks or points somewhere unexpected right after a file cleanup, check whether the cleanup moved the specific copy that icon was bookmarked to — the fix is having Emile remove and re-drag the icon from `/Applications`, not editing the Dock plist directly (safer, avoids risk of corrupting his Dock layout).

See also: [[personal/about-emile]], [[personal/cegep-school-organization]], [[feedback/proactive-file-organization]]
