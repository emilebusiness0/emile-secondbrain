---
name: cannot-save-pasted-images
description: Correction — on this Mac's Claude Code setup, images Emile pastes into chat DO land as real files in ~/Downloads; the original "no filesystem access" finding was wrong for this environment
metadata:
  type: feedback
  modified: 2026-08-20
---

**Correction (2026-08-20):** the original 2026-08-15 finding below was wrong for this setup. When Emile pasted 6 screenshots directly into chat (Meta Ads lead images, filenames IMG_5123–IMG_5128), all 6 turned out to be real files sitting in `~/Downloads/`, timestamped to the exact minute of the paste — confirmed via `ls -la ~/Downloads`. So on this Mac, pasting an image into the Claude Code chat client does save it to disk (in Downloads, under its original filename) as a side effect of the paste — it's not just an inline/ephemeral render.

**How to apply now:** when Emile pastes an image and a real file is needed (to move, rename, delete, attach elsewhere), check `~/Downloads` first for a file matching the expected name/timestamp before assuming no path exists. If found, treat it like any other local file — the deletion caveat still applies: never `rm`/permanently delete, only move to `~/.Trash` (Emile has asked for pasted-then-used images to be cleared out of Downloads afterward — do this via Trash, not a hard delete, since permanent deletion is off-limits). Only fall back to "no accessible file" (the original finding, still possibly true on other environments/setups) if a Downloads search comes up empty.

---

Original finding (2026-08-15, superseded above for this Mac): when Emile pastes a screenshot directly into the conversation (not as a file path, just an inline image), Claude Code can *see* and read the image content, but has no filesystem access to the underlying bytes — there's no local file to copy, move, or reference with any tool. Attempting to "save it as a file" without a real path produces a poor substitute (e.g. manually transcribing the image as text/markdown), which Emile found unreadable/ugly compared to the actual photo.

**Why the original finding mattered:** any request like "save this [pasted image] to my files" seemed impossible to fulfill directly. Silently substituting a transcription instead of flagging the limitation up front wastes a round trip and produces a worse result than what was asked for. This is now superseded by the correction above, but the underlying caution (don't invent a fake save, verify a real path first) still stands — just check Downloads before concluding no path exists.

See also: [[personal/mac-file-organization]], [[feedback/gmail-no-attachment-access]]
