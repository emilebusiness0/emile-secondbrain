---
name: project-duovert-backlink-campaign
description: "Duo Vert backlink/directory signup campaign status, NAP consistency fix, and revenue-crisis context behind it"
metadata: 
  node_type: memory
  type: project
  originSessionId: 555f7e4f-2052-42fd-bc87-be47a4294732
  modified: 2026-08-15
---

Started 2026-08-07. Site relaunched ~2026-08-03/05 and is ranking poorly (position 45-75 on
money pages like [[duo-vert/seo-history]] pages, despite real search demand) purely
because it's brand new — technical audit came back clean (no noindex, correct canonical,
valid schema, content depth already met). Real competitors (Interlock Masters, Pavage
Robillard, Paysagiste Charette, Sunrise Paysagiste) all have years of accumulated citations;
Duo Vert has almost none. Backlinks/citations are the actual lever, not more on-page work.

**Why: Émile has only had one client this summer and is under real financial pressure.**
SEO/backlinks cannot rescue this specific summer (too slow); paid Google Ads was proposed as
the fast-lead lever — the campaign was actually built in the Google Ads console 2026-08-09,
see [[duo-vert/google-ads-campaign]] for every setting and its current status (blocked on
Beckett adding his payment card).

**How to apply:** if Émile brings up ranking/revenue frustration again, don't re-litigate
whether backlinks matter — that's settled. Pick up the tracker below. Don't suggest asking
past clients for reviews again — he said he's already asked everyone who would give one.

## NAP inconsistency found and being fixed
Website/GBP phone (correct): **819-328-2129**. Facebook/Instagram/Yelp all showed the wrong
number **819-592-9595**. Business address: 74 Rue Félix-Leclerc, Gatineau, QC J9H 6Y2.
- Yelp: ✅ fixed 2026-08-07
- Facebook, Instagram: ✅ fixed 2026-08-10 — all three platforms now consistent. Note: Google's
  own cached search index still shows the old number on Facebook/Instagram/Yelp as of
  2026-08-13 (stale cache, not a real regression — the platforms themselves are correct).

## TWO files in the site repo, read both before resuming
- `~/Documents/Duo Vert/duovert-site/marketing/backlink-tracker.md` — signup **status** per site.
- `~/Documents/Duo Vert/duovert-site/marketing/backlink-opportunities.md` — **NEW 2026-08-18**, the
  researched/verified target list: what to sign up for next, in priority order, plus a
  competitor citation-gap table and a verified dead/not-worth-it list. See the research
  summary section below.

## Backlink opportunity research, 2026-08-18 (researched twice; French pass overturned the first)

**Émile's correction, which was right and is now a standing rule:** the first research pass
leaned too English. **~70% of Duo Vert's clients are francophone**, and Quebec search volume is
French-first, so directory/backlink research MUST be run with French queries (`annuaire`,
`bottin`, `répertoire`, `inscription gratuite`). The French pass surfaced directories the
English queries never returned AND overturned the top recommendation. See
[[feedback/french-first-quebec-research]].

**⛔ CORRECTION — Techo-Bloc "Techo-Pro" is NOT viable. Do not apply.** The first pass called it
the best find because there's no membership fee. That was wrong/incomplete. Reading the full
live requirements list shows a hard gate: **"Minimum $25,000 annual purchase of Techo-Bloc
products"** (entry tier is defined as $25k-$74,999/yr in purchases). Duo Vert does restoration,
not new installs, and doesn't buy pavers at that volume. Applying would just get a rejection.
Same for **Unilock** (needs 5+ years installing hardscapes, they have ~2). Net: **no
paver-manufacturer link is reachable right now.**

**🚨 TOXIC/DEAD DOMAINS FOUND — this is the most important takeaway.** Expired Quebec directory
domains have been bought by spammers, and they still appear in current, legitimate-looking
French listicles:
- `outaouaisdabord.ca` — was a real Le Droit-covered Outaouais buy-local platform, **now an
  online gambling/slots site**. NEVER submit.
- `novo411.com` — listed as a QC business directory, **now a Vietnamese adult site**. NEVER submit.
- `quebec411.com` — empty `/lander`, parked domain. Dead (was item #10 on the tracker).
- `quebec-entreprises.com` — DNS does not resolve. Dead.
- `zip411.net` — connection refused. `icriq.com` — free but broken SSL cert + industrial-only scope.
- Panier Bleu — ceased operations Feb 2024.
**Standing rule: never submit to a directory without loading it in a browser first.**

**✅ VERIFIED free + DOFOLLOW (checked rel attributes in the live DOM on competitor listings):**
AnuGo.ca, 411.ca, n49.com (free path is `/add-biz`, not the paid `/pricing`), MaCommunaute.ca
(Outaouais-specific, best *local* relevance), PME d'ICI (pmedici.ca). Note `rel="noopener
noreferrer"` is a security attribute and still passes value; only `nofollow`/`sponsored`/`ugc` don't.

**🔑 Structural finding: 411.ca = Pages Jaunes = YP, one funnel.** 411.ca's "Add your business"
points to `solutions.yp.ca/free-listing`. So the long-"stuck" Pages Jaunes item is actually
worth THREE directories in one submission, and 411.ca is dofollow with competitor Paysagiste
Leblanc already listed. Also loaded the form: it only collects name/phone/email/first/last, so
it's a lead-capture intake and **the sales callback is structural, not avoidable**. Resubmit in
business hours opening with « Je veux uniquement l'inscription gratuite, aucun forfait. » If
they refuse, finally close it as dead.

**❌ Verified not worth it — don't re-research:**
- Construction411.com and APCHQ's trouverunentrepreneur.com — both verified to carry **NO
  outbound website link** at all. APCHQ also requires paid membership.
- Les Pages Vertes — free tier exists but gives a **Facebook link only**; the website link is
  paywalled at $167.88/yr.
- Affaires411.ca — no website link visible. 411Gatineau.com — paid reseller/ad model.
- APPQ — ~$1,120/yr + $300 admin. BBB — no Quebec operation (already settled).
- **ID Gatineau is NOT a directory** (was tracker item #11): it's a business support org
  (financing/consulting/networking) with no bottin at all. Removed from the directory list.

**Best remaining upside is relationship links, not directories:** (1) **Maçonnerie Dépôt**,
444 rue Saint-Louis Gatineau, the local supplier carrying Permacon/Rinox/Techo-Bloc — ask in
French whether they keep an "installateurs recommandés" list, costs nothing and is hyper-local
plus industry-relevant; (2) **Bulletin Gatineau** local French community news, "deux jeunes
entrepreneurs de Gatineau" is a pitchable story; (3) **Techniseal Applicator Certification**
(1-day paid course, they already use the products, and unlike Techo-Pro there's no minimum
purchase gate) — ask whether it comes with a public listing.

**Paid but genuinely high-value, flagged separately since Émile said free only:** FC Gatineau /
AS Gatineau sports-club sponsorship ~$500/12mo, which includes a logo AND website link on the
club site. Genuinely local + relevant + in front of Gatineau parents (the actual buyer). If
Émile ever pays for one link, this is the one to pay for. Also: Chambre de commerce de Gatineau
(pricing unpublished, scales with headcount, worth one email) and Techniseal's 1-day paid
Applicator Certification (they already use Techniseal products).

**Judgment call recorded:** deliberately did NOT hand Émile a "top 200 free listing sites" dump.
Those lists are mostly nofollow, no-traffic sites that don't move rankings. The list was kept
short and ranked on relevance > locality > NAP consistency. If Émile asks for "more" backlinks,
the honest answer is that volume isn't the lever; Techo-Bloc + a local sponsorship + review
platforms are.

## RBQ licence question — RESOLVED 2026-08-18
**No RBQ licence is required for Duo Vert's work, and pursuing one is not a useful trust
signal.** Per RBQ's own guidance, pavers resting on gravel as part of landscape management are
exempt; a licence is only triggered once elements are attached to the building. This closes the
open question that was sitting in `backlink-tracker.md`. NEQ 3381922817 remains the available
registration trust signal. Summary as of 2026-08-07:
- Soumission Rénovation: ✅ done — activation confirmed via email 2026-08-10
- HomeStars: **blocked by a real signup bug** — their postal code field rejects J9H 6Y2 every
  time, reproduced twice months apart. Émile emailed service@homestars.com 2026-08-07 to
  report it. Don't have him retry the signup form until they reply — it's their bug, not his input.
- Yelp: done
- Apple Business Connect: verification email received 2026-08-13 ("been verified now"). Full
  profile completed same session: brand/logo added (Single brand flow), cover photo, About
  description (FR), 12 real project photos (all hero images + final-CTA image, resized — see
  photo-sizing note below), Actions button linking to `/soumission/`, a Showcase promoting the
  15% front+back-yard discount, category "Paving Contractor." All submitted, back to "In
  Review" (up to 5 more days). Verify later via the Apple Maps app or Siri search once approved
  — checked "Insights" tab in Business Connect for real traffic data starts appearing.
  **Apple photo-sizing requirements (reusable):** logo minimum 1024x1024 (square); cover photo
  minimum 1600x1040; "From the Business" photos minimum 720x960 (portrait). Site's existing
  images (logo 630x627, hero photos ~1376x768) were all too small/wrong-ratio — resized with
  `sips` (scale to cover the target box preserving aspect ratio, then center-crop to exact
  size) rather than AI upscaling, to avoid any risk of an upscaler subtly altering a brand logo.
- **BBB is NOT usable — Quebec isn't covered.** Found 2026-08-13: their province dropdown only
  lists Ontario and other provinces, no Quebec option. BBB doesn't operate a Quebec council/
  chapter at all (Quebec has its own consumer-protection body, OPC, not BBB). Drop BBB from
  this list entirely, don't attempt it again.
- Bing Places for Business: started 2026-08-13 (bing.com/places → bingplaces.com), found it
  auto-synced most info from the existing Google Business Profile already. Only manual fix
  needed was the About description, which pulled in English — translated to French matching
  site terminology (pavé-uni, scellant, sable polymère, aucun sous-traitant, soumission
  gratuite). Status of full signup completion not confirmed by session end.
- Nextdoor — flagged as a good channel (community trust/referral, not just an SEO citation)
  but not yet started as of 2026-08-13.
- 4 directories still fully not started: Houzz, Bark.com, Anugo, ID Gatineau (phone call).
  Réseau411 and Québec 411 status unclear, check the live tracker file. Pages Jaunes — attempted
  2026-08-08 via phone (dead-end sales pitch, no listing created), stuck; retry via
  solutions.yp.ca/free-online-listing directly instead of the phone line if resumed (see
  Session 2026-08-08 notes below)

Copy-paste business description/NAP text for all 12 sites lives in
`marketing/directory-listings.md` in the same repo — reuse it rather than redrafting.

## Working pattern that worked well this session
Émile drives the browser himself (site source now lives locally at
`~/Documents/Duo Vert/duovert-site` per [[duo-vert/memory-architecture]] update — no longer
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
