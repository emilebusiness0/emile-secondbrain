---
name: cannot-save-pasted-images
description: Claude Code cannot access or save the raw file of an image Emile pastes directly into chat — no filesystem path exists for it
metadata:
  type: feedback
  modified: 2026-08-15
---

Discovered 2026-08-15: when Emile pastes a screenshot directly into the conversation (not as a file path, just an inline image), Claude Code can *see* and read the image content, but has no filesystem access to the underlying bytes — there's no local file to copy, move, or reference with any tool. Attempting to "save it as a file" without a real path produces a poor substitute (e.g. manually transcribing the image as text/markdown), which Emile found unreadable/ugly compared to the actual photo.

**Why this matters:** any request like "save this [pasted image] to my files" cannot be fulfilled directly — there is no shortcut. Silently substituting a transcription instead of flagging the limitation up front wastes a round trip and produces a worse result than what was asked for.

**How to apply:** the moment Emile pastes an image and asks to save/keep/file it, say immediately that there's no accessible file for a pasted image — don't attempt a workaround (transcription, description, etc.) as if it satisfies the request. Give him the manual save step instead (right-click/save the image himself, then tell Claude Code where it landed so it can be moved/renamed/organized from there). Once a manual save produces a real path, everything downstream (moving, renaming, organizing) works normally.

See also: [[personal/mac-file-organization]]
