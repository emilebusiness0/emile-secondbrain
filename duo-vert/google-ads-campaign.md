---
name: duo-vert-google-ads-campaign
description: Google Search Ads campaign built 2026-08-09 — every setting/decision made during setup, draft-save gotcha, and what's still pending (Beckett's payment card)
metadata:
  type: project
  modified: 2026-08-28
---

Built 2026-08-09, in response to [[duo-vert/backlink-campaign]]'s revenue-crisis context (one client all summer, SEO too slow to help this season). Chose **Google Search Ads over Meta** — Émile ran Meta before at ~$35/lead, and Search Ads better fits his situation because he already has GSC data proving real search demand for his exact services, and his site converts well once people land (so the bottleneck is top-of-funnel traffic, not conversion).

## Every setting chosen, and why

**Goal / landing page:** "Leads" goal, Search campaign type. Landing page is `https://duovert.ca/restauration-pave-uni-gatineau/`, **not** the homepage and **not** the bare `/soumission/` form page — deliberately chosen as the middle ground: the restauration page has proof/trust content (process, testimonials, guarantee) plus its own "Demandez votre soumission" button into the form, so it builds trust before asking, without the friction of a bare form or the irrelevance of a general homepage for someone who already knows what they searched for.

**Conversion goal:** "Submit lead form" (not "Request quotes" — initially picked Request quotes as more literal, but Émile read Google's own description for "Submit lead form" which used "requesting a free quote for paving restoration" as its exact example, so switched). Paired with "Contacts" originally, but the form only allowed **one** goal to be selected — went with **Submit lead form alone**, since it's the trackable, structured pipeline already wired into the Google Sheet lead tracker, unlike calls/texts which have no clean attribution.

**Excluded services/categories:** deliberately did NOT build separate keyword/ad-group targeting around nivelage, nettoyage, or scellant as standalone services — Émile confirmed nobody hires Duo Vert for just sealing or just leveling, it's always bundled into a restoration project (matches [[duo-vert/company]]). Same logic removed Google's auto-suggested "Paysagisme et aménagement paysager" (landscaping, not their service) and "Prestations extérieures générales et commerciales" (too broad/commercial, they're residential-only) and "Revêtement de sol" (flooring — risks matching indoor flooring searches) from the business-category bubbles.

**Search themes / keywords (32 total):** built from real services + real cities + GSC-proven English demand. Includes English variants (interlock driveway/stones Gatineau, paver sealing Gatineau) because GSC data shows these already rank position 1-2 organically for real search volume, just with the zero-click French-snippet problem noted in [[duo-vert/seo-history]] — the ad itself can be in whichever language matches, sidestepping that snippet-mismatch issue entirely for paid traffic.

**Locations:** custom, not radius — service area is two disconnected clusters (Gatineau-side towns: Gatineau, Hull, Aylmer, Masson-Angers, Buckingham; specific Ottawa neighborhoods: Britannia-Bayshore, Island Park, Carlingwood-Woodroffe, Westboro-Hintonburg, Mechanicsville, The Glebe) that a single radius couldn't cleanly capture without either missing part of one cluster or overshooting into unserved parts of Ottawa. Removed Google's 4 auto-suggested locations (too far from actual service area). Targeting set to "presence" (physically in the area), not "presence or interest."

**Languages:** French and English both — English justified by the same proven organic English-query demand.

**Bid strategy:** Maximize conversions, no target CPA — deliberately not Target CPA/ROAS since those need ~15-30 conversions of real history to calibrate against and the account has zero; not Maximize conversion value since individual lead $ values aren't tracked in Ads (project values vary $600-4000, no per-lead value assigned).

**Budget: $18.70/day**, chosen specifically because Google's own forecast comparison showed it as the *most cost-efficient* tier, not just the cheapest:
- $18.70/day → ~2 conversions/week, ~$59/conversion
- $43.49/day (Google's "recommended") → ~3 conversions/week, ~$89.54/conversion
- $124.38/day → ~5 conversions/week, ~$152/conversion

Cost-per-lead climbs with budget because the account only has a limited pool of high-intent local searches available; spending more forces the algorithm into progressively worse/more competitive auctions to hit the higher spend target. Google's UI showed a yellow "budget lower than similar advertisers" nudge pushing toward $43.49 — flagged as a real conflict-of-interest (Google earns more the more he spends), not neutral advice. Émile initially leaned toward $43.49 for the extra volume (reasoning: Beckett back the next day = capacity to work more leads), talked through the tradeoff, ultimately chose $18.70 to start, with explicit plan to revisit after real performance data comes in.

**Promo:** took the "spend $600, get $600 in Ads credit" offer — flagged that at $18.70/day it takes ~32 days to hit the $600 spend threshold, so worth checking the promo's exact expiry window rather than assuming it applies.

**Automation toggles — turned OFF, all of them:**
- Text customization (Google auto-generating headline/description variants from site content) — off, to protect the specific fact-checked copy written this session (avoids resurrecting dropped/inaccurate claims like "sans fourmis" or the vague old guarantee wording).
- Final URL expansion (Google overriding the chosen landing page with another page on the site it predicts converts better) — off, to protect the deliberate restauration-page choice; also would risk sending scellant-keyword clicks to the scellant page, undermining the "don't advertise unbundled services" decision above.
- Image enhance/adjust and landing-page-sourced images — off, keeps only the specifically hand-picked real project photos (~12 images across formats), consistent with the "authentic, not AI-polished" positioning.
- Video: skipped entirely this launch — Émile tried downloading Instagram Reels for video assets but Instagram's own recompression degrades quality too much on re-export, and declined the AI-generate-from-images option as looking too generic/inauthentic for a hands-on trades business. Real source footage (not re-downloaded from Instagram) would be needed for a future video asset.

Turning off all four automation toggles dropped Google's "optimization score" — deliberately ignored, since that score partly measures compliance with Google's automation preferences, not actual expected performance, and every toggle turned off was a considered, specific decision.

**Ad copy, headlines, sitelinks:** full text (12 short headlines, 5 long headlines, 4 descriptions, 7 sitelinks with descriptions, CTA "Get quote") drafted this session — not reproduced in full here since it's easily regenerated from this file's decisions if the draft is lost, but the descriptions specifically excluded "sans fourmis" (unconfirmed claim) and use the bounded workmanship-only guarantee language (matches [[personal/website-build-playbook]]'s established guarantee wording), plus the real 15% front+back-yard discount (not a fabricated "$150 off").

## Draft-save gotcha (important if resuming)

Reached the payment step and **could not add a card** — it's Beckett's card, he was away, back 2026-08-10. Correctly did NOT attempt to enter any payment info. Google Ads normally auto-saves an in-progress campaign as a draft, but Émile's normal Google search for "Google Ads" from a separate personal browser landed on Google's public marketing page (not the actual logged-in console) and showed no sign of the draft — caused real confusion, worth remembering: **always navigate directly to `ads.google.com` while already signed into the correct business account (`duo.vert.gatineau@gmail.com`), never via a search-engine click-through**, which lands on the generic marketing/sales page regardless of account state.

What actually worked: copying the exact URL of the in-progress campaign-builder page (from the Claude Browser pane session) and pasting it into the other browser — this loaded the draft successfully. **Not yet verified whether this is a true account-level save or just a session-cookie artifact** — should have Émile sign out and back in, then retest the same URL, to confirm before relying on it with Beckett. If the link doesn't survive a sign-out/sign-in cycle, treat this file as the full rebuild spec instead — every decision above should be enough to recreate the campaign from scratch without re-deciding anything.

## Still open

- Confirm the draft URL survives sign-out/sign-in (untested as of session end).
- Beckett adds payment card, campaign actually launches. **His return date (2026-08-10) has
  passed with no recheck done as of 2026-08-15 (now 13 days stale)** — don't assume still
  pending, confirm directly.
- Once live: check cost-per-lead after 3-4 days (not a full month) as an early kill-switch check, given Émile's real financial pressure this summer.
- Revisit budget tier once real performance data exists — could scale to $43.49+ if $18.70 proves the concept and Beckett/Émile want more volume.

See also: [[duo-vert/backlink-campaign]], [[duo-vert/seo-history]], [[duo-vert/company]]
