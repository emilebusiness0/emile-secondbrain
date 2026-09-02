---
name: ask-detailed-specs-before-building
description: For structured deliverables and personal/professional content, ask what belongs in each section before drafting or entering it, don't fill things in unilaterally and present them after the fact
metadata:
  type: feedback
  modified: 2026-08-29
---

Ask what Emile wants in each section of a deliverable before writing or entering content, rather than drafting everything myself and presenting it as done. This applies to structured data (spreadsheet columns, form fields) and to personal/professional content (LinkedIn profile sections, bios, descriptions) alike.

**Why:** first observed with structured deliverables, where guessing at field/column meaning produced rework. Reinforced strongly 2026-08-27 during a live LinkedIn profile edit: Emile asked to "optimize" his profile, and without asking what he wanted said in each section, I drafted and saved a headline, About summary, Experience description, and industry directly to his live public profile. He interrupted to say this wasn't what he wanted, he wanted to be asked what belongs in each section first, then have me turn his input into strong, professional copy, not have me invent the content myself and publish it. This matters more for content that represents him publicly (a LinkedIn profile, professional bio) than for internal tools, since the content itself carries his voice and judgment, not just data structure.

**How to apply:** before writing copy for any section of a personal/professional profile, bio, or similar public-facing content, ask what Emile wants covered in that section (facts, tone, emphasis) and wait for his answer. Only after that, draft polished, professional prose myself, present it for review, and enter/save it only once he's approved it. Don't default to "I'll just write something reasonable and show you the result" for content that speaks in his voice. See [[feedback/no-em-dashes]] for a standing formatting rule that also applies to this kind of writing, and [[feedback/build-locally-not-live-browser]] for the adjacent (but distinct) preference about not driving live browser UI when a local file would do, this LinkedIn case is different because there's no local-file alternative, the live browser is unavoidable, but the ask-first principle still applies to content decisions.

**Reinforced 2026-08-28, extended to product/feature specs, not just content copy:**
during CRM prototype planning, I wrote a full architecture plan (stack, data model,
phasing) based on a few scope-level AskUserQuestion answers, without asking what he
actually wanted the product to look and work like — screens, sections, how integrations
should surface in the UI. He rejected it: "you haven't asked me like any questions on
what I want in it... I prefer that you ask... don't just invent stuff or create stuff
that I won't use and just waste my tokens and my time." He then gave a detailed,
feature-level spec unprompted (modeled on GoHighLevel's actual structure: lead
pipeline with search/filter by stage, a contacts section, integrations for
email/SMS surfaced on the lead detail view, a performance-tracking section) — meaning
the lesson isn't "ask fewer questions," it's "ask the *right* level of question": for a
build with real product shape (screens, sections, what lives where), ask about the
product shape itself, not just infra/scope-level decisions (where it runs, what data,
which integrations in the abstract). See [[duo-vert/custom-crm-prototype]] for
the specific spec he gave.

See also: [[duo-vert/website-build-overview]], [[personal/soccer-coach-website]],
[[duo-vert/custom-crm-prototype]]

**Reinforced again 2026-08-29** during the CRM visual-polish/mobile round: unprompted,
Emile said "I prefer replying to 20 questions now rather than fixing it five times
later, saves me tokens and time." Same lesson as above, now stated as an explicit
standing preference in his own words - default to asking upfront (via AskUserQuestion)
whenever a request has real ambiguity in scope or exact output (e.g. an exact filename
template, a UI behavior choice), rather than picking a reasonable default and risking a
redo.
