---
name: gmail-no-attachment-access
description: The Gmail MCP connector exposes attachment filename/mimeType/id only, not the actual bytes — can't view email image attachments, must ask user to paste them in chat instead
metadata:
  type: feedback
  modified: 2026-08-20
---

Discovered 2026-08-20: `get_thread`/`get_message` on this Gmail MCP connector return attachment metadata (filename, mimeType, an opaque `id`) but there's no companion tool to fetch the actual attachment bytes — no `get_attachment`/download call exists in the toolset. This means an email with image attachments (e.g. screenshots) can't be opened or read directly, no matter the `messageFormat` used.

**Why this matters:** any task that requires reading content *inside* an email attachment (photos, screenshots, scanned documents) can't be done via the Gmail tools alone.

**How to apply:** when a task needs the contents of an email's image/file attachment, say up front that the connector can't pull the bytes, and ask the user to paste the attachment directly into chat instead — pasted images can be read visually, and per [[feedback/cannot-save-pasted-images]] they typically also land as real files in `~/Downloads` on this Mac, so a save/cleanup step downstream works too. Don't waste a round trip trying `RAW` format or other workarounds first — this is a toolset gap, not a format issue.

See also: [[feedback/cannot-save-pasted-images]], [[duo-vert/sheets-tracking]]
