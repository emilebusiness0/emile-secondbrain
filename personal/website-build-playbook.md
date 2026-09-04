---
name: personal-website-build-playbook
description: Emile's reusable methodology for building websites with Claude Code — design defaults, SEO checklist, content process, workflow — built from the Duo Vert project, meant to carry forward to future client sites
metadata:
  type: user
  modified: 2026-09-01
---

Emile's current business is Duo Vert (paver restoration, Gatineau/Ottawa), but he's planning to build websites for other companies too (no agency/brand name yet as of 2026-08-05). duovert.ca was the first real site built end-to-end with Claude Code and is the reference implementation this playbook is distilled from. **The goal of this file: capture what stays the same across any client site — structure, SEO method, workflow, quality bar — so a new site starts from a strong default instead of re-deriving everything from scratch.** Update this file whenever a future site session produces a new reusable lesson, a preference correction, or something tried that didn't work. This is a living document, not a one-time snapshot.

## Design system defaults (the parts that transfer between brands)

Visual details (colors, fonts, imagery) are brand-specific and should NOT be copied wholesale from Duo Vert to a new client — only the *structural* patterns below are meant to be reused:

- **One `<h1>` per page, always.** H2 style pattern that reads well: `font-size: clamp(1.5rem, 3vw, 2.2rem); font-weight: 900; letter-spacing: -0.05em;`. Bold, slightly condensed headings (negative letter-spacing, heavy weight) read as confident/modern — this was Emile's preferred look on Duo Vert, worth offering as a default starting point on a new site rather than a generic weight-400 heading.
- **Hero sections must be `min-height: 100vh` + `min-height: 100dvh`** (declare both, dvh wins where supported). A hero at 80-85vh leaves a visible strip of the next section before any scrolling — reads as broken/unfinished on first load, especially on mobile. This was a real bug found and fixed across 17 pages on Duo Vert; start every new site's hero at 100vh/100dvh from day one instead of rediscovering this.
- **Grid layouts for card groups of 4: prefer 2×2 over 4-across.** Four thin tall columns read cramped; a 2×2 grid with roughly square cards reads more polished and is what Emile asked for after seeing both. Default to 2×2 (or whatever makes cards closer to square) for any "how it works" / process-step / feature-card section with an even card count.
- **FAQ accordion pattern:** `faq-item` / `faq-header` (button, toggles) / `faq-answer` (wrapper, `overflow: hidden` by default) / `faq-answer-inner` (actual text, padded). The `overflow: hidden` on the closed state is load-bearing — omitting it causes visible content jump/overflow bugs when opening.
- **Nav/topbar/footer: identical across every page of a site**, generated from one template/include if the stack allows it, or copy-pasted exactly if it's static HTML per page (Duo Vert's case — Vite + static pre-built HTML per route). Any inconsistency (e.g. one page's dropdown missing a link the others have) is a bug, not a style choice.
- **Floating fixed-position elements (e.g. a call button) need a scroll-collision check.** On Duo Vert this was found overlapping body text at various scroll positions on mobile and never fixed — check for this proactively on a new site rather than waiting for it to be flagged.

## SEO — the checklist to run on every page, every time

This is the concrete, always-do list mined from a full-site audit and cleanup pass on Duo Vert (2026-08-05). Apply it to every new page as it's built, not as a retrofit at the end:

1. **Every `<img>` has explicit `width` and `height`** (read the real file dimensions, don't guess) — prevents layout shift (CLS). Every image has real `alt` text.
2. **Canonical tag on every page.** Exactly one `<h1>`. Meta description ≤ 160 characters (longer gets truncated in Google's SERP).
3. **BreadcrumbList JSON-LD on every non-homepage page.** One canonical `LocalBusiness`/`Organization` entity via a shared `@id` (e.g. `https://example.com/#localbusiness`) referenced from every page's schema block, instead of each page declaring its own anonymous copy — this is what lets Google (and any consumer of the structured data) understand it's one entity, not N different businesses.
4. **Sitemap has `<lastmod>` on every URL**, kept current, resubmitted to Search Console after any content-heavy deploy.
5. **JSON-LD validity is non-negotiable** — after any batch of schema edits, re-parse every `<script type="application/ld+json">` block on the site with a real JSON parser before considering the work done. Don't eyeball it.
6. **FAQPage schema is fine to add for AI/LLM citation value, but do not expect a Google rich-result star/snippet from it** — Google restricted FAQ rich results to government/healthcare sites in August 2023. Don't spend time chasing "why isn't my FAQ showing in search" — it's a platform policy, not a bug.
7. **Review/AggregateRating schema on a business's own site will never produce a Google star-rating rich snippet either** — "self-serving reviews" have been excluded from rich results since 2019. The schema can stay (harmless, doesn't hurt), but don't sell it as an SEO win. Real star ratings in search only come from third-party platforms (Google Business Profile, Yelp, etc.).
8. **Location/city pages are a duplicate-content trap.** If a site has multiple location pages (this was the single biggest content issue found on Duo Vert), each one needs genuinely distinct content — not just the city name swapped into an identical template. Specifically check: is the FAQ different per location, not copy-pasted? Does the "why choose us" / context paragraph reference something real and specific to that location (a landmark, a demographic trait, a geographic feature) rather than generic boilerplate? A same-site page-similarity check (rough diff of visible text between two location pages) is a fast way to catch this — 88% textual similarity between two "different" pages was the red flag on Duo Vert.
9. **Content depth target for a cornerstone/service page: ~1500-1600 words of real, non-padded content.** This is a floor for topical coverage, not a rule to pad to — thin content under this on a page meant to rank for a competitive term is worth expanding with genuinely useful specifics (process detail, real numbers, FAQ depth), not filler sentences.

## Content-writing process (this is the part Emile cares most about getting right)

- **Never draft content with placeholder, generic, or competitor-researched numbers as a stand-in for the real fact.** Identify what facts a piece of content needs, ask the client to confirm those facts first, only then draft. If a number in existing content looks suspiciously generic/round/unconfirmed (e.g. a specific year range, a specific chemical name, a "90 cycles of X"), flag it and ask rather than assuming it's right or silently rewriting it — sometimes generic-looking numbers turn out to be confirmed-accurate (verify before "fixing").
- **Content must read like a human wrote it, not like it was machine-translated or templated.** Watch for: grammar agreement errors, mismatched apostrophe styles (curly vs straight) within the same file, leaked words from the source language if translating, awkward literal phrasing, and — the biggest one — a sentence/paragraph that's byte-identical across multiple pages except one swapped-in variable (city name, product name). That last pattern is *always* worth fixing even if grammatically correct, because it reads as generated and is genuinely bad for SEO (duplicate content across pages meant to rank independently).
- **Marketing copy: avoid open-ended, disputable claims.** Emile explicitly rejected a "100% satisfaction guarantee" in favor of a guarantee scoped to objective workmanship defects ("a real defect, not a matter of taste") — his reasoning: open satisfaction guarantees invite endless subjective disputes a business can't win. Default to specific, bounded, defensible claims over sweeping ones in any client's guarantee/promise language.
- **Legal/policy pages (terms, privacy policy, contracts) are hands-off for proactive edits — always ask first, even for a real, confirmed error.** See [[feedback/legal-content-needs-permission]] for the incident this came from. This is a hard rule, separate from the general content-quality process above.
- **For a full-site proofreading/content-audit pass on a multi-page site, split the page list into groups and run parallel subagents, each told to read full files (not excerpts) and report concrete file+quote findings only, no generic style opinions.** This was much faster than a sequential read-through of 27 pages and each agent surfaced genuinely different findings — a reusable pattern for any future full-site QA pass, not just Duo Vert's.

## Workflow / tooling

- **Edit the actual source files locally with git, not through a live browser-based site builder** — see [[feedback/build-locally-not-live-browser]]. Directly relevant reason from the Duo Vert history: an AI Studio-based workflow repeatedly and silently corrupted binary image files; local file edits don't have this failure mode.
- **Verify every visual change across desktop, tablet (768px), and mobile (375px)** before calling it done — not just desktop. Measure computed styles directly (e.g. hero element height vs. `window.innerHeight`) rather than eyeballing a screenshot when precision matters; screenshots are for catching things measurement can't (layout collisions, overlap, visual polish).
- **Build and deploy: for a Vite (or similar) static-build stack, always run the actual build command and deploy the build output folder (`dist/`), never the raw source repo.** Dragging the whole source folder (node_modules, src, config files) onto a host is both wasteful and unreliable — path mapping that a build step performs (like Vite's `public/` folder overlay) isn't guaranteed without it. Also: when deploying manually to a host like Netlify, double- and triple-check which site/project you're dropping the folder onto — a wrong-folder-onto-wrong-site mistake caused a real production outage on duovert.ca (traced in [[duo-vert/website-build-overview]]). If a project sits in the same parent directory as other small unrelated deploy targets, that's a standing risk worth actively avoiding (separate directories, clearly named).
- **Before considering a multi-page change "done," run a verification pass that checks the specific things fixed, not just spot-checks.** E.g. after fixing a grammar bug across 11 files, grep for the old (wrong) string across the whole site to confirm zero remaining instances, not just re-check the files remembered. This caught a real regression once (a revert-and-reapply operation on 3 files silently reintroduced an already-fixed unrelated bug in the shared nav).

**Discovery-call framing, added 2026-09-01 (from the soccer coach project,
first real outside client):** Emile's own useful distinction for reading a
client during discovery - they'll fall into one of two modes, and the
right approach differs:
1. **Precise-vision client** - has a specific look/feel in mind. The job
   is extracting it accurately (reference sites, exact style words,
   specific must-have pages/features) rather than substituting his own
   taste.
2. **Vague-idea client** - has a rough sense of what they want but no
   concrete vision. Here Emile's own creative judgment should drive the
   actual design decisions, using the discovery answers as constraints
   (audience, goal, business specifics) rather than a spec.
Worth identifying which mode a client is in early in the call, since it
changes how much design latitude to take.

## Open / not-yet-solved

- Floating call-button-over-text overlap on mobile scroll (Duo Vert) — identified, not fixed as of 2026-08-05, worth solving properly (once) and then treating the fix as a default for future sites too.
- No established image-compression workflow yet — Emile mentioned wanting this as a standard skill for future sites; nothing concrete tried/learned yet, flag for the next site where it comes up and document what works here.

## See also
[[duo-vert/website-build-overview]] (concrete history this playbook was distilled from), [[duo-vert/design-system]], [[feedback/legal-content-needs-permission]], [[feedback/build-locally-not-live-browser]], [[feedback/fix-root-cause-not-just-instance]]
