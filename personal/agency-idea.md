---
name: personal-agency-idea
description: "Emile's idea (2026-08-31) to turn his Claude-Code website/CRM building skills into a paid service for other businesses"
metadata:
  type: project
---

**Started 2026-08-31, still an idea, not a decision or a committed venture.**
Emile had a realization after finishing the Duo Vert CRM's Round 10 build:
what he's built for his own paving company (a production website plus a
custom CRM with real Meta/Gmail/GA4/Search Console integrations, per
[[duo-vert/custom-crm-prototype]]) is agency-quality work, and he thinks he
could sell the same thing to other businesses as a one-person shop using
Claude Code — websites, CRMs, and "maybe other stuff" not yet identified.

**His own pricing instincts, stated in chat, not yet tested against a real
client:** websites $1000-2000 depending on size (vs. $10k+ for some
"professional" agency sites), a CRM add-on he guessed at $800-1000/month but
wasn't sure about.

**Feedback given (not just encouragement — real pushback, per
[[feedback/reasoning-and-pushback]]):**
- The core insight is real and already proven — Duo Vert's own build is a
  legitimate case study, not hypothetical. This is a real pattern other solo
  devs are doing with AI coding tools.
- Building is not the bottleneck, finding clients is — that's untested.
- The CRM is not a quick upsell: per-client integration setup (their own
  Google Cloud OAuth client, their own Meta Business app, etc.) repeats real
  friction Emile personally hit multi-session debugging during the Duo Vert
  build (see the Meta OAuth saga in [[duo-vert/custom-crm-prototype]] Round
  6). This is the biggest hidden cost of the model, not the coding itself.
- Hosting other businesses' customer data makes Emile a data processor for
  them — flagged as needing a real contract/liability conversation before
  taking on a real client, not something to wing.
- Timing risk against Duo Vert's own 2026-2027 season prep (sales crew
  hiring, RBQ cert, brand work — see [[duo-vert/season-2027-plan]]) — floated
  that this might fit better as an off-season winter project than something
  run in parallel with the busy season.

**Hosting architecture recommendation given (his specific question, "how
would we host CRMs for other companies"):** don't build one shared
multi-tenant platform (single app/DB serving every client) — that's real
SaaS-company engineering and too big a bet before proving demand. Instead
treat the Duo Vert CRM codebase as a template, fork a small isolated
deployment per client (own Vercel/Netlify project, own Turso DB, own env
vars) — same low-cost hosting pattern already researched for Duo Vert itself
(see [[duo-vert/custom-crm-prototype]] "Real risks before real team use").
Revisit real multi-tenancy only once several clients are actually paying for
it.

**Pricing framing given:** website pricing instincts ($1-2k) are reasonable
and well-positioned against agency quotes. Recommended NOT treating the CRM
as a one-time flat add-on — price it as a monthly retainer ($50-150/mo
hosting+support suggested, on top of a build fee) instead of a single
$800-1000 bump, both for recurring revenue and to actually cover ongoing
hosting/maintenance cost.

**Client-acquisition framing given:** avoid Fiverr/Upwork-style marketplaces
(race to the bottom). Better channels: warm network first (his soccer coach
lead, see [[personal/soccer-coach-website]], is exactly this and floated by
Emile himself as a possible CRM upsell too, not just a website client);
trade/local-service businesses specifically (landscapers, cleaners,
contractors) since Emile understands their operational pain firsthand;
Duo Vert itself as the portfolio/case study.

**Recommended next step, not yet acted on:** don't build a second product
right now — treat the soccer coach as the real test case for how a second
client's build (and a real CRM pitch) actually goes, rather than planning
further in the abstract.

**Follow-up (2026-08-31, same day):** Emile confirmed he has real winter
bandwidth for this — his own read is that Duo Vert's 2027 season prep won't
take the whole winter (contradicts the earlier assumption in this file that
timing might compete with season prep; his own assessment, not independently
verified). He also asked directly whether there's more than CRM+site he
could offer.

**Adjacent-service ideas given, all grounded in things already built for
Duo Vert (not generic brainstorming):**
- **Ads management** (Meta ads tracking, the mostly-built Google Ads
  campaign — see [[duo-vert/google-ads-campaign]]) — real CPL/ROI visibility
  most agencies selling this don't actually have.
- **SEO/local search** (GBP, schema, city pages, backlinks — see
  [[duo-vert/seo-history]], [[duo-vert/backlink-campaign]]).
- **Automations layer** (auto-reply, review-request pattern, the digest
  trend feature from [[duo-vert/custom-crm-prototype]] Round 9-10) —
  flagged as possibly the strongest differentiator of all of these, more
  than the CRM itself: custom-wired automation tailored to one business is
  rare and hard for generic tools (HubSpot/GHL) to replicate, and clients
  often half-abandon the generic version.
- **Reporting/analytics as a standalone offering** — the digest/trends work
  could be sold on its own (wiring a business's existing GA4/ads/CRM
  together into a weekly summary) without a full CRM build, much cheaper to
  deliver than the whole CRM.

**Packaging advice given:** avoid becoming a "does everything" shop —
framed as a ladder, not a menu: website first (cheap, proves the
relationship) → CRM second (once real operational trust exists) → ads/SEO/
automations as the ongoing retainer layer. Reinforced the same
"soccer-coach-as-real-test, not more upfront planning" recommendation from
the first round of this idea.

**Long-term scaling thought (2026-08-31, same conversation):** Emile floated
hiring employees if this grows enough. Pushback given: his actual edge is
being personally 5-10x faster via Claude Code, which is what lets him
undercut $10k agencies at $1-2k — hiring doesn't preserve that automatically
unless new hires work the same AI-accelerated way, otherwise he just becomes
a smaller version of the agencies he's underpricing. Recommended hiring be
the answer to "more paying clients than I can serve solo," not a growth goal
pursued for its own sake — template/automate further first.

**Real uncertainty voiced (2026-08-31, same conversation):** Emile admitted
he's not sure where this is going yet, called it new/uncomfortable territory
(read positively, as a sign of real exploration, not just anxiety), and
named the actual worry directly: "I'd have to learn and once you know it,
well you always do it for me, but we have to have a base system that works."
Read as two distinct concerns worth keeping separate if this comes up
again: (1) does Emile need to learn to code himself, and (2) does the
process depend too much on live improvisation with Claude each time rather
than something repeatable.

Response given: concern (1) isn't the real risk — the scarce skill in this
model is judgment/taste (catching bad specs, insisting on real verification,
knowing what "good" looks like for a client), which Emile has already
demonstrably been doing throughout the CRM build, not raw coding ability.
Concern (2) is the legitimate one — most of what's been built so far exists
as decisions made live in conversation, not written down anywhere reusable.
Recommended next concrete step (not yet started): turn this into an actual
documented playbook, extending the existing
[[personal/website-build-playbook]] pattern to cover the CRM/automations
side too, so the process doesn't depend on re-deriving every decision from
scratch with each new client.

**Sharper version of the dependency worry (2026-08-31, same conversation):**
Emile refined the concern from "do I need to code" to something narrower and
real: for judgment-heavy offerings like ad recommendations/automations
strategy, if he can't independently tell right advice from wrong, and can't
correct Claude when it's wrong, bad advice could genuinely reach a paying
client. His own framing: "we would have to learn together and make sure
what we do actually helps the client."

Response given, treated as correct rather than smoothed over:
- Split the risk in two: checkable claims (does the site render, does the
  auto-reply send, does a computed number match a manual count — testing
  already covers this) vs. genuine judgment calls (raise this ad budget,
  fix this targeting) which are inherently uncertain even for a human
  expert and where a wrong call costs the client real money.
- Pointed to the digest trend feature itself
  ([[duo-vert/custom-crm-prototype]] Round 10) as the concrete existing
  technique for the first category: code computes the real number, AI only
  phrases an already-triggered fact, never invents a judgment untethered
  from data. Confirmed this caps risk for reporting but does NOT fully solve
  it for actual strategic recommendations.
- Recommendation: before selling "ad recommendations" as a paid service to
  a stranger, build a real track record on Duo Vert's own ad account first
  — did suggested changes actually move CPL over a real stretch of time,
  not just sound plausible. Until that track record exists, sell the
  honest lower-risk version (factual reporting/monitoring) and hold off on
  strategic recommendations as a paid offering.

See also: [[duo-vert/custom-crm-prototype]], [[personal/soccer-coach-website]],
[[duo-vert/season-2027-plan]], [[personal/website-build-playbook]],
[[duo-vert/google-ads-campaign]], [[duo-vert/seo-history]],
[[duo-vert/backlink-campaign]], [[feedback/reasoning-and-pushback]].
