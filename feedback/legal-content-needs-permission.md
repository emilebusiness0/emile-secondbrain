---
name: feedback-legal-content-needs-permission
description: Don't edit legal/policy page wording proactively, even to fix real errors — ask first
metadata:
  type: feedback
---

Never edit the wording of legal pages (terms of service, privacy policy, contracts) proactively, even when a genuine error is found (leftover template boilerplate, factual/jurisdiction contradictions, language errors). Flag the issue clearly and ask before touching the text.

**Why:** During a full-site content audit of duovert.ca (2026-08-05), a proofreading pass found real problems in the legal pages — leftover "Airbnb/Uplisting" boilerplate from whatever template the pages were built from, a genuine contradiction (one page cited Ontario law/courts, another cited Québec, for a Gatineau QC business), and a few English words leaked into the French privacy policy. These were fixed inline as part of a broader "fix everything you find" pass. Emile then said explicitly: don't touch the legal pages' wording, revert exactly as it was — even though the issues were real. He was fine keeping non-content technical SEO additions (schema, breadcrumbs) on those same files, just not wording changes. Legal/policy text is treated differently from marketing copy — likely because it may have been reviewed/drafted deliberately, or changing it carries different risk than a typo on a service page.

**How to apply:** Before editing terms of service, privacy policy, contracts, or similar legal/compliance text on any project — even for objectively correct fixes (contradictions, leftover placeholder text, language errors) — report the finding and ask first rather than fixing proactively. This is a categorical rule for this content type, separate from the general "confirm facts before writing marketing content" habit ([[personal/website-build-playbook]]). If already fixed without asking, and the user asks for a revert, use git (`git checkout -- <file>`) against the last clean commit if available, then re-apply only the parts they confirm they want kept (e.g., unrelated technical/SEO additions) on top of the reverted original wording.
