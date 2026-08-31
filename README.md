---
name: emile-secondbrain-index
description: Index of everything in this vault
---

# Emile Secondbrain

This is Emile's shared knowledge vault — read by Claude Code at the start of every session, and (once connected) by Claude Cowork too. Curated facts only, not raw conversation transcripts — see [[duo-vert/memory-architecture]] for why. Not limited to Duo Vert — any topic gets its own section as it comes up.

## Personal (general, not Duo Vert-specific)

- [[personal/about-emile]] — general facts, preferences, other projects (separate from Duo Vert business/site facts)
- [[personal/dev-environment]] — what's installed/authenticated on his Mac (Homebrew, gh CLI, Playwright MCP) for Claude Code to use directly
- [[personal/golf-hobby]] — left-handed, beginner golfer building out a Wilson Ultra set
- [[personal/website-build-playbook]] — reusable methodology for building client websites (design defaults, SEO checklist, content process), distilled from the Duo Vert build
- [[personal/agency-idea]] — idea (2026-08-31, not decided) to sell website/CRM builds to other businesses using Claude Code, pricing/hosting/client-acquisition thinking
- [[project-current-todo-list]] — active cross-project to-do list

## Duo Vert — business & ops

- [[duo-vert/company]] — the business: services, pricing, area, team
- [[duo-vert/sheets-tracking]] — the 3-sheet back office plan (leads, expenses, clients/revenue); see also `Excalidraw/lead-webhook-pipeline.excalidraw.md` for a visual diagram of the lead pipeline
- [[duo-vert/soumission-template]] — confirmed Google Doc quote template, pricing rules, Drive API gotchas
- [[duo-vert/memory-architecture]] — how this vault works, why curated over raw-dump, the Code/Cowork sync gap
- [[duo-vert/backlink-campaign]] — directory/citation signup campaign (revenue-crisis context), NAP consistency fix
- [[duo-vert/google-ads-campaign]] — Google Search Ads campaign built 2026-08-09, every setting chosen and why; decided skipped for the 2026 season (2026-08-18), Meta ads performing better instead — full spec kept for a next-season revisit
- [[duo-vert/print-collateral]] — door-to-door print kit (flyer/door hanger/business card), design system settled 2026-08-11, flyer still mid-revision
- [[duo-vert/custom-crm-prototype]] — testing a self-built CRM with Claude Code, parallel to (not replacing) the decided GoHighLevel plan; started 2026-08-28

## Duo Vert — website build (duovert.ca, ~6 months of history)

- [[duo-vert/website-build-overview]] — project status, source-file location, page status, launch checklist
- [[duo-vert/design-system]] — hero CSS values, service/city page structure, review inventory, verification checklist
- [[duo-vert/ai-studio-playbook]] — Gemini/AI Studio prompt patterns, city-page prompt sequence, common mistakes (legacy workflow — current workflow is local editing, see website-build-overview)
- [[duo-vert/photo-workflow]] — image generation workflow, naming, the AI Studio image-corruption saga and its resolution
- [[duo-vert/seo-history]] — audit findings, content expansion rounds, mobile audit findings, prepared keyword/competitor research docs
- [[duo-vert/bilingual-site]] — EN mirror of duovert.ca (24 page pairs + legal), built and deployed live 2026-08-29

## Diagrams

- `Excalidraw/lead-webhook-pipeline.excalidraw.md` — visual diagram of the lead pipeline (Netlify Form → Proxy → Apps Script → Sheet), see [[duo-vert/sheets-tracking]]
- `Excalidraw/Drawing 2026-08-01 22.36.47.excalidraw.md` — untitled, no description anywhere in the vault; purpose unknown, candidate for deletion if Emile confirms it's not needed

## Feedback (how Emile wants work approached)

- [[feedback/build-locally-not-live-browser]] — prefer local deliverable files over driving live browser UIs
- [[feedback/proactive-vault-saving]] — catch and save significant moments without being asked; Code writes directly, Cowork flags for Emile to relay
- [[feedback/proactive-vault-reading]] — bias toward opening a vault file when in doubt about relevance, so advice/opinions are informed by it, not just direct topic questions
- [[feedback/reasoning-and-pushback]] — explain reasoning, push back instead of agreeing by default, verify claims scaled to stakes
- [[feedback/browser-verification-token-cost]] — prefer text extraction over screenshots when verifying browser state
- [[feedback/label-vault-file-mentions]] — append "(emile-secondbrain)" after any vault filename mentioned in chat
- [[feedback/vault-cross-reference-integrity]] — propagate moved facts/new files to all referencing files + README in the same edit
- [[feedback/rename-move-verification-checklist]] — on any rename/move, check both "still functions" AND "every identity registry updated" before declaring done
- [[feedback/fix-root-cause-not-just-instance]] — every fix must also prevent recurrence, not just patch the one instance
- [[feedback/legal-content-needs-permission]] — never edit legal/policy page wording proactively, even to fix a real error — ask first
- [[feedback/lead-outreach-in-person-visit]] — propose an in-person visit directly in outreach messages, not a call or photos first
- [[feedback/sms-draft-formatting]] — draft SMS/text messages inside a fenced code block, not plain markdown paragraphs
- [[feedback/client-defect-flagging-ask-dont-quote]] — raise unrelated defects as a question + defer, don't bundle a fix into the quote client asked for
- [[feedback/french-first-quebec-research]] — research Quebec markets with French search terms first; ~70% of Duo Vert's clients are francophone and WebSearch is US-indexed
- [[feedback/gbp-post-content-style]] — GBP posts: no sales pitch, 3-5 sentences, no dashes, sound human, build around real photos
- [[feedback/avoid-subagents-for-hands-on-builds]] — for work Emile is actively directing, do it directly, don't spawn Agent/Plan/Explore subagents

---

*New topics get their own file + a line here as they come up. Keep this index short — detail lives in the linked files, not here.*
