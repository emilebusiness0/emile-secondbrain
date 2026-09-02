---
name: walkthrough-direct-steps
description: When walking Emile through an external platform's UI, give direct numbered click-by-click steps, not clarifying questions
metadata:
  type: feedback
  modified: 2026-08-29
---

When guiding Emile through a third-party website/dashboard he's unfamiliar with
(Meta Business Manager, Google Cloud Console, etc.), default to direct,
simple, numbered "click this, then this" instructions rather than asking him
what he sees and waiting for an answer before giving the next step.

**Why:** During Meta Business verification setup (2026-08-29,
[[duo-vert/custom-crm-prototype]] Round 6 follow-up), asked "what do you see
when you click into Security Center?" instead of just telling him where to
click. He responded "im so confused, just tell me what i gotta do" — a clear
signal the back-and-forth question style added friction instead of helping.

**How to apply:** For unfamiliar external UIs, front-load a complete numbered
sequence (with a direct link when possible) instead of a single step + a
question. Only ask a follow-up question if a step is genuinely ambiguous
without seeing his screen (e.g., multiple buttons could apply) — and even
then, describe what to look for rather than open-endedly asking "what do you
see."
