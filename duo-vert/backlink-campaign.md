---
name: project-duovert-backlink-campaign
description: "Duo Vert backlink/directory signup campaign status, NAP consistency fix, and revenue-crisis context behind it"
metadata: 
  node_type: memory
  type: project
  originSessionId: 555f7e4f-2052-42fd-bc87-be47a4294732
  modified: 2026-08-10
---

Started 2026-08-07. Site relaunched ~2026-08-03/05 and is ranking poorly (position 45-75 on
money pages like [[duo-vert/seo-history]] pages, despite real search demand) purely
because it's brand new — technical audit came back clean (no noindex, correct canonical,
valid schema, content depth already met). Real competitors (Interlock Masters, Pavage
Robillard, Paysagiste Charette, Sunrise Paysagiste) all have years of accumulated citations;
Duo Vert has almost none. Backlinks/citations are the actual lever, not more on-page work.

**Why: Émile has only had one client this summer and is under real financial pressure.**
SEO/backlinks cannot rescue this specific summer (too slow); paid Google Ads was proposed as
the fast-lead lever but deferred until Beckett (co-founder) is back — draft campaign saved at
`marketing/google-ads-draft.md` in the site repo, ready to launch when they decide to spend.

**How to apply:** if Émile brings up ranking/revenue frustration again, don't re-litigate
whether backlinks matter — that's settled. Pick up the tracker below. Don't suggest asking
past clients for reviews again — he said he's already asked everyone who would give one.

## NAP inconsistency found and being fixed
Website/GBP phone (correct): **819-328-2129**. Facebook/Instagram/Yelp all showed the wrong
number **819-592-9595**. Business address: 74 Rue Félix-Leclerc, Gatineau, QC J9H 6Y2.
- Yelp: ✅ fixed 2026-08-07
- Facebook, Instagram: still wrong as of session end 2026-08-07 — pick up here next session

## Backlink signup tracker — full live status in the site repo
`/Users/emilemorissette/Documents/duovert-site/marketing/backlink-tracker.md` — read this file
first before resuming, it has per-site status and exact next steps. Summary as of 2026-08-07:
- Soumission Rénovation: submitted, pending ~2 days — check back 2026-08-09
- HomeStars: **blocked by a real signup bug** — their postal code field rejects J9H 6Y2 every
  time, reproduced twice months apart. Émile emailed service@homestars.com 2026-08-07 to
  report it. Don't have him retry the signup form until they reply — it's their bug, not his input.
- Yelp: done
- Apple Business Connect: submitted 2026-08-09, both verification methods (domain via Cloudflare
  TXT record, business registration via NEQ 3381922817 — see reusable-fact note in the live
  tracker file for the legal-name/NEQ details), up to 5 business days for review, check back
  ~2026-08-14
- 8 more directories not yet started (Pages Jaunes, BBB, Houzz, Bark.com, Anugo, Reseau411,
  Québec 411, ID Gatineau phone call)

Copy-paste business description/NAP text for all 12 sites lives in
`marketing/directory-listings.md` in the same repo — reuse it rather than redrafting.

## Working pattern that worked well this session
Émile drives the browser himself (site source now lives locally at
`~/Documents/duovert-site` per [[duo-vert/memory-architecture]] update — no longer
missing from this Mac) and reads form fields aloud; I tell him exactly what to paste from the
prepared kit. Faster than me trying to browser-automate logged-in third-party sites I have no
credentials for — account creation/login is a hard boundary I don't cross even with permission.

## Session 2026-08-08 updates

**Pages Jaunes / YP.ca is stuck, not live.** Émile called their signup line and got a sales
pitch for a paid 12-month plan instead of a free listing; confirmed via `site:` search that
no "Duo Vert" listing exists on either pagesjaunes.ca or yp.ca. Pages Jaunes and Yellow Pages
are the same company/database in Canada (one covers both) — real user reports online confirm
their phone-intake path is a known lead-gen tactic that often doesn't produce the free listing
it advertises. If resumed: try solutions.yp.ca/free-online-listing directly (skip the phone
call). Lower priority now given the wasted call — don't push Émile back to this one first.

**GSC re-checked 2026-08-08, one day after the Yelp fix + Soumission Rénovation submission:**
essentially flat (too early for any citation work to be crawled/indexed yet — that takes
1-3+ weeks minimum, expected and not a bad sign). Money pages (gatineau, nivelage, faq,
restauration) still sitting position 45-61.

**Real finding: Duo Vert DOES rank top 1-5 for several queries already**, but with a specific
pattern worth remembering — hyper-local exact-match phrases (e.g. "restauration de pavé uni")
and English-language "interlock" terms ("interlock driveway gatineau" pos 2.0/68 impr,
"interlock stones gatineau" pos 1.0/22 impr) rank very well, but **get zero clicks** despite
the good position. Likely cause: page titles/meta descriptions are French-only, so an
English-language searcher sees a French snippet and skips it even at position #1. This is an
open, easy, code-level fix (English title/meta variants on key pages) that doesn't need to
wait weeks like the backlink work does — proposed to Émile, not yet actioned as of session end.

**Also explained to Émile: personal phone-search rank checks are unreliable** — even in
incognito, IP-based geolocation still boosts local results when searching from within Gatineau
itself, and map-pack results (GBP-driven) are a completely separate ranking system from GSC's
organic page-position tracking. Don't let Émile's own phone searches override GSC data as the
source of truth for how the site is actually ranking nationally/broadly.

**Open thread: Émile mentioned getting "a couple new reviews"** during this session but I
never got the count or which platform — worth asking directly next time rather than assuming
it's on GBP. Didn't get to verify current review count.

## GSC "clicks going down" false alarm (2026-08-09) + branded-search spike explained

Émile got worried clicks were declining after seeing GSC's Page Indexing report (a "page with
redirect" flag on the homepage, several pages "discovered — currently not indexed") and after
clicking a "Validate Fix" button. Pulled real daily data via the connected GSC tool: **no
actual decline** — impressions climbed steadily through early August (102 → 165 by Aug 8) and
the two best click days of the month were the two most recent complete days (Aug 7: 4, Aug 8:
5). The apparent drop was just Aug 9's partial/incomplete day data (GSC always lags 1-3 days)
being misread as a trend. **Method note, reusable:** always check the actual daily table before
treating a scary-looking single data point as a trend — same lesson as the "average position
degrading" false alarm from [[duo-vert/website-build-overview]]'s 2026-08-05 session.

Also explained: "page with redirect" and "discovered — currently not indexed" are both normal,
non-error states for a new site (redirect = Google found a non-canonical URL variant, harmless;
not-indexed = Google hasn't prioritized crawling it yet, expected given low domain authority).
Clicking "Validate Fix" only asks Google to recrawl and confirm a fix — it does not affect
rankings or traffic itself, cannot have caused any decline.

**Real finding, separate from the false alarm:** a July 17-18 spike to 10-11 clicks/day (vs the
normal 0-5) traced via query-level breakdown to **branded search** ("duo vert," "duo vert
gatineau" at 66-100% CTR, position ~1-2), not a generic-keyword ranking improvement. A branded
spike means someone already knew the business by name that day and looked it up directly —
word of mouth, a business card handed out, a social post, a referral — not an SEO/content
change. Émile didn't recall a specific trigger for that date when asked. **Reusable takeaway
for Émile's core frustration** ("I have 1600-word pages, great reviews, a great backend, why am
I still ranked 15-24 on money pages?"): on-page content quality and off-page authority
(backlinks, citations, domain age) are separate ranking factors — his on-page work is genuinely
done, the remaining gap is purely the years-long backlink/citation head start his real
competitors (Interlock Masters, Pavage Robillard, etc.) have. Not a sign anything is broken or
being done wrong; it's the expected shape of a ~1-week-old domain competing against
multi-year-old ones on that specific axis.
