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
- [[personal/mac-file-organization]] — Documents/<Domain>/ folder structure for keeping the new Mac organized (full restructure 2026-08-26)
- [[personal/cegep-school-organization]] — Emile's Cégep course list (Automne 2026) and the Documents/Cegep/<Course>/ file system
- [[personal/soccer-coach-website]] — future freelance website for Emile's old soccer coach, blocked on Duo Vert season ending
- [[personal/agency-idea]] — idea (2026-08-31, not decided) to sell website/CRM builds to other businesses using Claude Code, pricing/hosting/client-acquisition thinking
- [[project-current-todo-list]] — active cross-project to-do list

## Duo Vert — business & ops

- [[duo-vert/company]] — the business: services, pricing, area, team
- [[duo-vert/sheets-tracking]] — the 3-sheet back office plan (leads, expenses, clients/revenue); see also `Excalidraw/lead-webhook-pipeline.excalidraw.md` for a visual diagram of the lead pipeline
- [[duo-vert/soumission-template]] — confirmed Google Doc quote template, pricing rules, Drive API gotchas
- [[duo-vert/memory-architecture]] — how this vault works, why curated over raw-dump, the Code/Cowork sync gap
- [[duo-vert/backlink-campaign]] — directory/citation signup campaign (revenue-crisis context), NAP consistency fix
- [[duo-vert/google-ads-campaign]] — Google Search Ads campaign built 2026-08-09, every setting chosen and why; decided skipped for the 2026 season (2026-08-18), Meta ads performing better instead — full spec kept for a next-season revisit
- [[duo-vert/revenue-growth-plan]] — 5-advisor council verdict + priority list for getting leads before season end (2026-08-12); final 2026 tally: 5 jobs for the summer
- [[duo-vert/print-collateral]] — door-to-door print kit (flyer/door hanger/business card); flyer and business card signed off 2026-08-15, door hanger route still being scouted
- [[duo-vert/employee-hiring-plan]] — hiring door-to-door sales reps + cleaners for the 2027 season, compensation structure, candidate list
- [[duo-vert/season-2027-plan]] — 2027 off-season brainstorm: UGC ad talent, storage unit, brand identity, crew training, RBQ cert, GoHighLevel CRM idea
- [[duo-vert/custom-crm-prototype]] — self-built CRM with Claude Code, parallel to (not replacing) the decided GoHighLevel plan; through Round 10 (2026-08-31)

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
- [[feedback/cannot-save-pasted-images]] — pasted images DO land in ~/Downloads on this Mac (corrected 2026-08-20) — check there before assuming no file exists
- [[feedback/edit-in-place-no-version-sprawl]] — overwrite the existing version across revision rounds, don't pile up numbered copies
- [[feedback/no-em-dashes]] — never use em dashes in anything written for Emile
- [[feedback/sms-draft-formatting]] — draft SMS/text messages inside a fenced code block, not plain markdown paragraphs
- [[feedback/client-defect-flagging-ask-dont-quote]] — raise unrelated defects as a question + defer, don't bundle a fix into the quote client asked for
- [[feedback/french-first-quebec-research]] — research Quebec markets with French search terms first; ~70% of Duo Vert's clients are francophone and WebSearch is US-indexed
- [[feedback/gbp-post-content-style]] — GBP posts: no sales pitch, 3-5 sentences, no dashes, sound human, build around real photos
- [[feedback/avoid-subagents-for-hands-on-builds]] — for work Emile is actively directing, do it directly, don't spawn Agent/Plan/Explore subagents
- [[feedback/ask-detailed-specs-before-building]] — ask what belongs in each section/screen before drafting, don't invent and present after the fact
- [[feedback/no-measurements-to-clients]] — Duo Vert doesn't volunteer exact site measurements to clients unprompted, shares if directly asked
- [[feedback/gmail-no-attachment-access]] — Gmail MCP connector can't fetch attachment bytes, ask Emile to paste images in chat instead
- [[feedback/proactive-file-organization]] — sort every touched file immediately; new top-level domain vs. subfolder heuristic
- [[feedback/no-inventing-when-citing-sources]] — when reporting from a source, state only what's literally written, say "not mentioned" instead of guessing
- [[feedback/plain-text-over-markdown-for-documents]] — outside Obsidian: prose gets .txt, structured/row data gets a real .xlsx, never raw markdown
- [[feedback/walkthrough-direct-steps]] — give numbered click-by-click steps for unfamiliar external UIs, don't ask "what do you see" first

---

*New topics get their own file + a line here as they come up. Keep this index short — detail lives in the linked files, not here.*
