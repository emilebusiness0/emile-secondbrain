---
name: duo-vert-bilingual-site
description: "English-mirror translation project for duovert.ca — approved plan, slug map, toggle mechanism, and build progress"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-31T17:29:33.080Z
  originSessionId: 4e03382c-91a5-4b0a-8fc4-b044b6d1ddd9
---

Emile approved (2026-08-28) a full English mirror of every French page on duovert.ca, with a FR/EN toggle switch in the header, to rank for English "interlock"/"pavers" searches in Ottawa/Gatineau. Plan file: `~/.claude/plans/ok-lets-do-the-joyful-lightning.md` (may not survive across sessions — this memory is the durable record).

**Architecture decision:** matches [[duo-vert/website-build-overview]]'s existing pattern exactly — no shared templating, each of the 29→58 pages is a fully self-contained duplicated static HTML file (same convention already used site-wide). Emile explicitly chose this over introducing a build/partial system, to stay consistent with how the site already works.

## Slug map (FR → EN), approved, no `/en/` prefix except homepage

| Page | FR | EN |
|---|---|---|
| Homepage | `/` | `/en/` |
| Restauration | `/restauration-pave-uni-gatineau/` | `/interlock-repair-gatineau/` |
| Nettoyage | `/nettoyage-pave-uni-gatineau/` | `/interlock-cleaning-gatineau/` |
| Scellant | `/scellant-pave-uni-gatineau/` | `/interlock-sealing-gatineau/` |
| Nivelage | `/nivelage-pave-uni-gatineau/` | `/interlock-releveling-gatineau/` |
| Gatineau/Ottawa/Aylmer/Hull/Buckingham/Masson-Angers (city hubs) | `/<ville>/` | `/interlock-pavers-<ville>/` |
| 6 Ottawa quartiers | `/ottawa-<quartier>/` | `/interlock-pavers-ottawa-<quartier>/` |
| À propos | `/a-propos/` | `/about-us/` |
| Avant/Après | `/avant-apres/` | `/before-after/` |
| Tarifs | `/tarifs/` | `/pricing/` |
| Contact | `/contact/` | `/contact-us/` |
| FAQ | `/faq/` | `/interlock-qa/` — titled "Questions & Answers" not "FAQ" (FR already uses the untranslated acronym as its own slug, so EN spells it out instead of colliding on the same word) |
| Soumission | `/soumission/` | `/free-quote/` (+ `/free-quote/thank-you/`) |
| Référence | `/reference/` | `/referral-program/` (+ `/referral-program/thank-you/`) |
| 3 legal pages | `/politique-confidentialite/`, `/conditions-utilisation/`, `/conditions-contrat/` | `/privacy-policy/`, `/terms-of-use/`, `/contract-terms/` — **translate but hold out of the "ready to publish" set**, Emile explicitly wants legal sign-off before these go live |

## Toggle + persistence mechanism (built and verified working)

- Small two-segment pill widget (`FR | EN`, active side styled solid, inactive side is a real `<a>`) in the header's right-side cluster (desktop, class `lang-toggle-desk`) and mirrored in the mobile drawer (class `lang-toggle-mobile`) — CSS block `.lang-toggle*` added once per file, same as every other duplicated style block.
- **In-session consistency:** every internal href on an EN page points to EN sibling slugs (nav, mobile drawer, footer, CTAs, breadcrumbs); FR pages keep FR links. This alone means clicking anything after landing on an EN page keeps you in English.
- **Cross-visit persistence:** clicking the toggle does `localStorage.setItem('duovert_lang', 'fr'|'en')`. Only the **two homepages** (`/` and `/en/`) carry a redirect-on-load script (in `<head>`, before other tags) that reads this and `window.location.replace()`s to the other homepage if it doesn't match — deliberately scoped to homepage-only to avoid hijacking a specific-keyword Google search click or a paid ad landing on an interior page. The redirect script also explicitly skips firing when the URL has `gclid`/`fbclid`/`msclkid`/`utm_source` params, so Google/Meta ad clicks always land on the exact page the ad pointed to regardless of stored preference.
- hreflang triples (`fr-ca`/`en-ca`/`x-default`) added to both pages of every pair; `x-default` and the FR-only fallback point at the French version (primary market).
- New EN Netlify forms get a distinct `name` suffixed `-en` (e.g. `soumission-accueil-en`) plus fresh `data-netlify="true" netlify-honeypot="bot-field"` (safe — this is a new-form registration, not the attribute-removal gotcha noted in [[duo-vert/website-build-overview]]).

## Build progress — COMPLETE (2026-08-29)

All 24 content-page pairs, both `/merci` → `/thank-you` pairs, and all 3 legal-page translations are built and verified. `sitemap.xml` updated (27 → 51 URLs at this point, since merci/thank-you pages and the 3 held-back legal EN pages were still excluded here). **Superseded same day by the 2026-08-29 update below: Emile approved shipping the legal pages too, and the full site (including all 3 legal pairs, 54 URLs) was built and deployed live to Netlify — nothing is still "local only" as of 2026-08-29.**

Every page pair went through the same verification pass before being marked done: grep scan for leftover-French visible copy (CSS comments in French are expected and harmless — they exist pre-translation on every page and aren't user-visible), a tag-balance check (all pages carry one known pre-existing `div` off-by-one from the original FR source, unrelated to this project — accept it), and `json.loads()` validation on every JSON-LD block, then a live curl smoke-test of both URLs against the local dev server before wiring the FR-side reciprocal hreflang/toggle and adding the EN slug to `vite.config.ts`'s `staticRoutes`.

**Critical bug found and root-caused (session of 2026-08-28/29):** the shared `apply_slug_map()` function never mapped bare `href="/"` — meaning every early EN page's logo link still pointed at the French homepage instead of `/en/`, silently breaking the core "stay in your language" requirement. Fixed by adding a dedicated `fix_logo_href_to_en()` step (safe only on EN pages, where every bare `/` legitimately means the EN homepage) to the shared library, then batch-retrofitted the ~21 pages already built at that point. Every build script after that point calls it explicitly. **Lesson for future site-wide fixes:** when a bug is found in a shared helper after several pages already used it, fix the helper AND batch-recheck every already-built page — don't just patch the one file where it was noticed (see [[fix-root-cause-not-just-instance]]).

**Legal-page build gotcha:** for the 3 legal pages (privacy policy, terms of use, contract terms), the generic `apply_misc()`/`apply_footer()` passes translate short recurring phrases like "Politique de Confidentialité" globally — including mid-sentence inside the still-French legal body paragraphs, corrupting them (e.g. "Cette Politique de Confidentialité décrit" → "Cette Privacy Policy décrit"). Fix: for any page with a large hand-translated body block, swap the whole FR body → EN body **before** running the generic passes, not after — the generic passes are safe to run on top of already-English content but not on text still awaiting translation.

**UPDATE 2026-08-29: Emile approved deploying the legal pages live too** ("no dont care the legal can go live too") — this overrides the earlier hold-for-sign-off plan. Added the 3 legal pairs to `sitemap.xml` (54 URLs total now) with reciprocal hreflang. Full site (`npm run build` → drag `dist/` to Netlify) was deployed with everything included, no held-back pages. Note for future legal-content requests: this was a deliberate in-the-moment call by Emile to ship a translation of already-approved FR legal text, not a general waiver of [[feedback/legal-content-needs-permission]] — still ask before editing legal/policy wording on other occasions.

**Final URL/slug map, all built:**
- Homepage: `/` ↔ `/en/`
- 4 service pages: restauration/nettoyage/scellant/nivelage ↔ interlock-repair/cleaning/sealing/releveling-gatineau
- 6 city hubs: gatineau/ottawa/aylmer/hull/buckingham/masson-angers ↔ interlock-pavers-<ville>
- 6 Ottawa quartiers ↔ interlock-pavers-ottawa-<quartier>
- 5 utility pages: a-propos/avant-apres/tarifs/contact/faq ↔ about-us/before-after/pricing/contact-us/interlock-qa
- soumission ↔ free-quote (+ thank-you pair), reference ↔ referral-program (+ thank-you pair)
- 3 legal pages (held back, not in sitemap): politique-confidentialite/conditions-utilisation/conditions-contrat ↔ privacy-policy/terms-of-use/contract-terms

**Next step for Emile, not yet done by me:** review the pilot and legal pages in the browser, confirm EN copy/tone reads well, get legal sign-off on the 3 held-back pages, then decide when to `npm run build` and deploy to Netlify — nothing pushed live yet.

See also: [[duo-vert/website-build-overview]], [[duo-vert/design-system]], [[duo-vert/seo-history]], [[fix-root-cause-not-just-instance]]
