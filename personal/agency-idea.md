---
name: personal-agency-idea
description: "Emile's idea (2026-08-31) to turn his Claude-Code website/CRM building skills into a paid service for other businesses"
metadata:
  type: project
  modified: 2026-08-31
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

**New adjacent-service idea floated (2026-09-01):** an AI phone
receptionist — answers a business's incoming calls, sounds natural/
"human," and can book appointments — as a sellable product on top of
websites/CRM. Not yet scoped technically (voice model, telephony provider,
booking integration all undecided) or priced. Same pattern as the rest of
this idea: test on Duo Vert's own incoming calls first before pitching it
to anyone else, per the ladder/testbed logic already established above.

**Technical architecture walked through (2026-09-01, same day):** the
basic loop is speech-to-text -> Claude generates a response -> text-to-
speech, same shape regardless of implementation. Two build paths:
(1) roll-your-own - Twilio Voice for telephony, Deepgram for speech-to-
text, ElevenLabs for natural-sounding text-to-speech, wired together
manually - more control, more to maintain; (2) an all-in-one voice-agent
platform (Vapi, Retell, Bland AI as of this date) that bundles all four
pieces behind one API and a system prompt - faster to stand up, another
vendor dependency. Leaning toward roll-your-own eventually to match the
"own it, don't rent it" pattern already set by building a custom CRM
instead of buying GoHighLevel, but platform-first is the honest faster
way to prove the idea works before investing in custom plumbing.

**The real integration point, not just "answer the phone":** the agent
needs function-calling access into the same CRM already built - create a
Contact, log an Activity, check the Calendar model and book a slot
directly, reusing the exact data model the digest/tracking features
already use (see [[duo-vert/custom-crm-prototype]] Round 18). Also needs
a clean human-handoff path (transfer the call, or log a message as an
Activity for follow-up) for anything it can't handle - without that, one
bad call costs a lead instead of gaining one.

**Cost structure, why this stays "wire up, don't turn on":** metered per-
minute, not a flat fee - roughly $0.10-0.30/minute all-in (phone number +
STT + LLM tokens + TTS), whether platform-bundled or itemized DIY. Same
rule as Sent for SMS: build the plumbing now if it comes to that, don't
fund/activate it until it's actually going live (see
[[feedback/no-paid-setup-before-ready-to-use]]). Scope estimate: wiring
voice-agent actions into the existing Contact/Activity/Calendar model is
comparable complexity to the tracking feature just shipped in Round 18 -
a real build, not a weekend project, but not a huge one either given how
much of the CRM's data model it can reuse as-is.

**Reusability across future clients, asked directly (2026-09-01, same
day):** confirmed this is built once and templated, not rebuilt from
zero per client - same pattern as the website playbook and the CRM's
per-client fork model. Reusable: the actual pipeline (telephony wiring,
STT->Claude->TTS loop, the function-calling framework). Per-client: their
own phone number, a system prompt written around their specific business,
and connecting the function calls to their own CRM instance (same "own
OAuth app" friction already true for CRM integrations).

**Concrete starting recommendation given:** platform-first with **Vapi**
specifically (as of this date) rather than DIY, because the hard part of
"sounds genuinely human" is latency/interruption-handling, which the
platforms have already solved - that's the real engineering, not
something to redo from scratch just for control. Steps: sign up, connect
a Twilio number, write a system prompt (business facts + call-handling
behavior), wire one function call into the CRM's Contact/Activity model,
test-call it repeatedly and refine from the real transcripts.

**Correction to the cost-caution framing, same conversation:** unlike
Sent (all-or-nothing account funding), voice platforms are pay-per-
minute - a handful of test calls to himself costs pennies, not a real
commitment. So "wire up, don't turn on" doesn't need to apply as strictly
here as it did for SMS; a small cheap experiment is genuinely different
from funding a whole channel, worth not over-applying the same caution
everywhere by default.

**Real pricing researched (2026-09-01), not estimated:** Vapi's platform
fee starts at $0.05/min, but that's only the orchestration layer -
speech-to-text (~$0.01/min), the language model (~$0.02-0.20/min), and
text-to-speech (~$0.04/min) are billed separately on top, landing the
realistic all-in cost at **$0.07-0.25/minute**, past $0.30/min with
premium voices/bigger models. A phone number is ~$1/month. Illustrative
monthly cost at real volume: ~$10-40/mo at 50 calls/month (3 min avg),
~$40-150/mo at 200 calls/month - genuinely cheap even at real usage,
confirming the point above that the earlier caution was about not paying
*before* going live, not about ongoing cost being large once it is.
Sources: [Vapi Pricing 2026](https://emitrr.com/blog/vapi-pricing/),
[2026 Twilio Voice Pricing](https://quiq.com/blog/twilio-voice-pricing/).

**DIY cost researched and compared (2026-09-01), same conversation -
revises the earlier "lean roll-your-own eventually" framing:** priced out
Twilio (~$0.0085-0.014/min + ~$1/mo number) + Deepgram STT
(~$0.0077/min) + Claude (~$0.02-0.10/min) + ElevenLabs conversational TTS
(~$0.08-0.10/min) = **roughly $0.12-0.22/minute all-in** - essentially
the same range as the Vapi platform cost above, not meaningfully
cheaper. Corrected conclusion: DIY doesn't save real money at this
stage, it trades convenience for control (voice tuning, no vendor
lock-in, full pipeline visibility) at the same price point, plus real
engineering risk (stitching 4 APIs together, keeping latency low enough
to sound natural). Revised recommendation: platform-first (Vapi) is the
right call for a first build on cost grounds too, not just speed -
DIY only makes sense once something specific is actually needed that the
platforms can't provide, not as a default "own it" preference. Sources:
[Deepgram Pricing 2026](https://deepgram.com/pricing), [ElevenLabs
Pricing 2026](https://elevenlabs.io/pricing/agents).

**Real constraint surfaced (2026-09-01): Duo Vert's business number is
Emile's own personal number**, listed in many places already (GBP,
website, business cards) - switching it seemed complicated to him.
**Resolved, not actually a blocker:** call forwarding (a standard carrier
feature, not porting) solves this - the AI answers on a separate Twilio
number behind the scenes, and Emile's real number forwards to it, so
nothing listed anywhere needs to change. Recommended **no-answer/busy
forwarding** over always-forwarding specifically for him, since the
number is still his personal line too - his phone rings first as normal,
the AI only catches what he doesn't answer (mid-job, driving, after
hours), rather than intercepting every call.

**Billing model for reselling this, asked directly (2026-09-01):** how
companies selling a templated CRM+AI-caller product actually handle the
underlying per-minute cost. Answer: there's no way to make the cost
disappear (even large platforms pay Deepgram/ElevenLabs/an LLM provider
under the hood) - the reseller just holds the account and controls what
the end client sees. Two real models: (1) bundle a minute allowance into
a flat monthly fee priced to cover actual cost + margin, with overage
billed separately past that; (2) pure per-minute markup. Recommended (1),
folded into the same $150-300/mo CRM+site retainer already discussed
above rather than a separate line item - given the real cost data above
(~$10-40/mo at 50 calls), a generous included-minutes allowance still
leaves real margin inside that retainer range.

**Voice quality explained (2026-09-01)** - Emile had noticed some AI
phone voices sound genuinely human and others sound robotic, asked why.
Three real levers, not one: (1) the TTS engine itself - cheap/default
TTS sounds flat, ElevenLabs is the current standard for natural-sounding
speech (voice cloning is also possible there, for a fully custom/brand
voice later); (2) latency/interruption-handling - even a great voice
sounds broken with laggy responses or no ability to handle being talked
over, which is what a platform like Vapi is built to solve; (3) prompt
writing - short natural phrasing sounds human, stiff scripted phrasing
sounds robotic regardless of voice quality. All three need to be right
together; the bad ones he's heard are almost always failing on one or
more of these, not an inherent AI-voice limitation.

**Final step-by-step sequence for whenever he's ready to actually build
it** (not started, explicitly queued): (1) sign up for Vapi, pick an
ElevenLabs voice; (2) connect a new Twilio number through Vapi; (3) set
up no-answer/busy call forwarding from Duo Vert's real (personal) number
to it; (4) write the system prompt - business info, tone, what to
collect, hand-off rules; (5) wire one function call into the CRM's
Contact/Activity/Calendar model; (6) test-call it repeatedly, refine the
prompt from real transcripts; (7) turn forwarding on for real once it
sounds right.

**Instant outbound callback idea (2026-09-01) - possibly more valuable
than the inbound version above.** Emile asked about auto-calling every
new lead within seconds instead of only answering inbound calls. Real
stats confirmed (not the vaguely-remembered "50%" he cited): contacting a
lead within 5 minutes gives 21x higher qualification rates than a 30-min
wait, 78% of customers buy from whichever company responds first, a
5-min delay makes losing the lead 10x more likely, average business
response time is 42 hours industry-wide. **Technically straightforward
given what's already built**: `sendAutoReply` in
[[duo-vert/custom-crm-prototype]] already fires an instant email/SMS the
moment a new lead is created (website form or manual add) - adding an
outbound AI call as a third channel at that same trigger point is a
natural extension of existing architecture, not new plumbing. Flagged as
possibly the single highest-leverage use of the whole phone-assistant
idea, worth building alongside or even before the inbound-answering
version. Sources: [Lead Response Time Statistics
2026](https://caseyresponse.com/blog/lead-response-time-statistics),
[Speed to Lead Statistics](https://www.kixie.com/sales-blog/speed-to-lead-response-time-statistics-that-drive-conversions/).

**Billing consolidation question, asked directly (2026-09-01):** Emile
wanted to know if the shared team-SMS number (Sent) and the AI-calling
phone number could be bought from the same company, to avoid two
separate monthly bills. **Answer: no, not fully.** Sent is text-only
(SMS/WhatsApp/RCS per their own channel list, confirmed via their docs
during the SMS-tracking build - see [[duo-vert/custom-crm-prototype]]
Round 18) - it has no voice-calling capability at all, so it structurally
can't be the AI-calling provider. Vapi can buy/manage the phone number
directly though, so the AI-calling side collapses into one bill (Vapi
covers number + STT + LLM + TTS together) rather than a separate raw
Twilio account on top. Realistic outcome: two bills total (Vapi + Sent),
not one - each internally consolidated. **The only way to true one-bill
billing:** Twilio itself supports voice AND SMS on the same account, so
dropping Sent and moving team texting onto Twilio directly (same
account as the AI-calling number) would get to one vendor - flagged as a
real but bigger decision, since Sent is already built into the CRM
(working code, not just planned), not something to switch over just to
save one line on an invoice without weighing Sent's actual product
quality first.

**Why Sent got used in the first place, checked against the actual build
history (2026-09-01):** not a deliberate best-in-class pick - it was the
SMS provider already connected as a Claude Code tool when the CRM build
started, used for that reason of availability, not a feature comparison.
Emile had already voiced uncertainty about keeping it during the
auto-reply build (Round history, [[duo-vert/custom-crm-prototype]]) - the
SMS-send call was deliberately isolated into one function specifically so
a provider swap later would be cheap, anticipating this exact question.

**Twilio feature-parity confirmed (2026-09-01), not guessed:** Twilio
supports SMS, WhatsApp, and RCS with delivery/read-status tracking on
WhatsApp and RCS the same way Sent's lifecycle events work - plain SMS
has no read-receipt capability on either platform, a protocol limitation
of SMS itself, not vendor-specific. Real tradeoff: Twilio's API is more
raw/lower-level than Sent's purpose-built wrapper, but this matters less
in practice since the CRM's own database (not the vendor) already does
the real "shared inbox" unification work. **Emile is leaning toward
consolidating onto Twilio** (texting + AI calling on one account/bill) if
pricing comes out similar to Sent - not yet decided, wants a live price
comparison first. Sources: [Twilio RCS Business
Messaging](https://www.twilio.com/docs/rcs), [Twilio WhatsApp Business
API](https://www.twilio.com/en-us/messaging/channels/whatsapp).

**Real pricing pulled (2026-09-01):** Twilio SMS in Canada is
$0.0079/message (send + receive) + a small variable per-message carrier
fee + ~$1/mo for the number - straightforward flat per-message pricing.
Sent's model is structured differently, not just a different number - a
$0.015 "per-contact" platform fee plus pass-through carrier costs on top
- genuinely hard to do an apples-to-apples comparison without checking
Duo Vert's actual Sent dashboard/plan directly. **Combined monthly
estimate given for texting + AI calling together**, at Duo Vert's
realistic volume (~700 texts/mo, ~50 AI calls/mo at 3 min avg): texting
~$6-8/mo (Twilio) + AI calling ~$10-40/mo (Vapi) = **roughly $16-48/mo,
likely landing ~$20-30/mo** - close to the "$12/mo" ballpark Emile
originally hoped for, not exact but in the right range, especially once
consolidated onto one vendor instead of two. Sources: [Twilio SMS Pricing
Canada](https://www.twilio.com/en-us/sms/pricing/ca), [Sent
Pricing](https://www.sent.dm/en/pricing).

**Context as of 2026-09-01:** Duo Vert's field season is essentially done
(some 2027 prep left, but not much), so Emile is entering a stretch with
real free time — friends are back in school (cégep, see
[[personal/cegep-school-organization]]) while he still has slack, which he
explicitly wants to fill with this work rather than idle. Reiterated
strong personal conviction/urgency about pursuing the agency idea now
rather than later; treat this as motivation context, not a scoping
decision — the open questions above (client acquisition being the real
bottleneck, contract/liability for hosting client data, pricing untested)
are all still unresolved and still the right next things to press on
before assuming direction. See [[personal/motivation-and-mission]] for the
deeper why behind this drive.

**Operational framework given (2026-09-01), during real prep for the
soccer coach discovery call — first time these questions got concrete
answers instead of staying abstract:**

- **Hosting architecture:** Emile's own instinct was that hosting a
  client's site under his personal account felt wrong, and didn't want to
  hand over his own account password either. Confirmed that instinct and
  gave the real-agency pattern: a separate hosting account (his own
  Netlify/Vercel "agency" account, distinct from the personal one tied to
  Duo Vert), one project per client inside it, clients never get login
  access. The one exception: the **domain** should be registered in the
  client's own name/account, even though hosting sits under Emile's
  agency account — protects the client from being locked in if the
  relationship ends, protects Emile from becoming a bottleneck he doesn't
  need to be. Not yet set up (deferred, per
  [[feedback/no-paid-setup-before-ready-to-use]] — free tier, no cost
  blocker, just not done yet).
- **Pricing structure:** Emile wanted a consistent, defensible unit
  instead of guessing a number per client (worried about e.g. a mid-size
  site costing more than a large one just from inconsistent guessing).
  Recommended a 3-tier system priced by page count + feature complexity
  (the closest equivalent to surface-area pricing for a paving job):
  Starter (3-4 pages, no booking/payment) $600-900; Standard (5-7 pages,
  booking/contact system, testimonials) $900-1500; Custom/Advanced (8+
  pages, online payment/booking, custom design/animation) $1500-2500+.
  Flat hosting/maintenance retainer on top regardless of tier, $15-30/mo.
  The soccer coach site is expected to land in Starter or low Standard.
- **Payment collection:** recommended Interac e-transfer as the
  standard/expected method for freelance work this size in Quebec (no
  processing fees, everyone already has it) — a written summary
  (amount, scope, deposit/balance split) sent by text/email as the
  informal record, deposit collected before starting, balance collected
  at launch. No invoicing software needed at this scale.
- **Duo Vert's own site valued as a market comparison (2026-09-01):**
  asked directly what an agency would charge for the Duo Vert site itself.
  **Corrected page count, same day:** Emile confirmed the real count is 24
  French pages, 48 total counting the English mirror (not the ~15/~30
  first estimated, which came from an outdated skill description, not
  verified page-by-page). Estimate given: $4,000-6,000 CAD for the website
  alone (real technical SEO — schema/sitemap/GSC/GA4 — image pipeline,
  soumission system), separate from the custom CRM (valued separately at
  $5,000-15,000 build or the $50-150/mo retainer already discussed in this
  file). Given the corrected, larger page count, this estimate likely
  sits at or above the top of that range rather than the middle — not
  recomputed precisely, flagged as directionally higher. Explicitly above
  the Custom/Advanced tier ceiling above — flagged as a sign the 3-tier
  structure may need a fourth, higher bucket for bilingual/deep-SEO/
  multi-feature scope, and as a legitimate case-study number for pitching
  bigger future prospects.
  Caveated honestly, not just flattery: part of the low actual cost was
  Claude Code doing the heavy lifting (the same edge being sold to future
  clients, not something every build replicates this cheaply), and part
  was Emile already having full domain knowledge of his own business with
  no discovery call needed — advantages a stranger client won't provide.

**Website+CRM bundle pricing given (2026-09-01):** Emile asked what a
full bundle — same complexity as his own Duo Vert build (48-page-
equivalent bilingual site + the real custom CRM with Meta/GA4/Gmail/
Search Console integrations and digest/trend AI reporting) — would sell
for to an unrelated company, personalized to them. Estimate given: **build
fee $8,000-12,000** (not a straight sum of the site's ~$5,500-7,000 and
the CRM's ~$5,000-10,000 standalone values — real overlap from one
deployment/onboarding/design system justifies bundling below the sum),
**retainer $150-300/month** (covers hosting + CRM upkeep + tweaks, higher
than the $50-150/mo floated earlier for CRM alone since it now covers
both systems). Framed as genuinely good margin because a business buying
this piecemeal (an agency for the site + GoHighLevel/HubSpot-style SaaS
for the CRM) pays comparable or more, forever, for a rented system instead
of an owned custom one.

**Pushback given on his "gets faster each client" reasoning, same
message:** agreed the structural/design/SEO patterns genuinely do transfer
and speed up repeat builds — that part of his instinct is correct. But
flagged that the CRM's *integration* layer does not get faster the same
way: every client needs their own Google Cloud OAuth app and Meta
Business app approval, which has already cost real multi-session time on
his own build (see [[duo-vert/custom-crm-prototype]]) — budget real time
for that at client #5 or #20 too, not just client #1.

**Recommended NOT charging the $8-12K figure for the first bundled
client** — treat it as the steady-state price once one bundle is
delivered and demoable, price sale #1 below it as the same kind of
proof-of-concept logic already applied to the soccer coach's site.

**Recurring-vs-one-time services clarified (2026-09-01):** Emile got
overwhelmed thinking about managing multiple clients at once and asked
which services actually require ongoing monthly/weekly work vs. a single
build. Clarified: one-time = the website build, the initial CRM build,
initial SEO/schema setup. Recurring = GBP posting/review replies (weekly-
ish), reporting/digest delivery (weekly or monthly), CRM hosting/
maintenance/tweaks (ongoing), any future ads management. This distinction
matters for pricing (retainer vs. flat fee, already reflected in the
pricing framework above) and for workload planning as client count grows.

**"Agency ops CRM" idea raised (2026-09-01), same conversation:** Emile
floated building his own internal tool to track his agency's own clients
and their recurring work (who's due for a GBP post this week, whose
monthly report is due) - essentially reusing the same digest/tracking
architecture already built for [[duo-vert/custom-crm-prototype]], but
pointed at his own client base instead of Duo Vert's leads. Good idea in
principle, structurally identical to something he's already built once.
**Explicitly recommended NOT building this now** - at 3-4 clients a
simple tracking sheet (same lightweight pattern as the existing Duo Vert
leads sheet) is sufficient; flagged as another instance of new-build
temptation delaying getting an actual first paying client (same pattern
named in [[feedback/proactive-opinions-and-next-steps]]). Revisit once an
actual sheet starts breaking under real client volume, not before.

**Confirmed, same conversation:** Emile said he'll "100%" build this once
he reaches that point - reasoning it's good practice, low time cost, and
more efficient than alternatives. Agreed with the timing (matches the
"once the sheet breaks" threshold above) and with the time-cost read: an
ops-CRM tracking his own clients/service/due-dates doesn't need the OAuth/
Meta Business API integrations that made the client-facing CRM's
per-client setup slow (per [[duo-vert/custom-crm-prototype]]) - it's
internal data he already owns, so it's a genuinely faster build than the
integration-heavy work the earlier caveat was about.

**Third-party account onboarding process worked out (2026-09-01):** Emile
raised the practical worry, informed by his own real experience connecting
Duo Vert's Meta/Gmail accounts (see [[duo-vert/custom-crm-prototype]]
Round 6 Meta OAuth saga), that async back-and-forth ("I sent the link,
did you click it, check again") would be too slow when doing this for a
client's own accounts instead of his own. Resolution given: neither Google
(Gmail/GA4/Search Console) OAuth nor Meta Business Partner Access ever
require the client's password - both are one-click "Allow"/approve
actions on the provider's own screen. The actual fix for the friction is
process, not technology: **do all account authorizations live in one
scheduled screen-share session** (20-30 min), walking through each
connection one at a time while the client clicks Allow in real time,
rather than sending links async and waiting on replies. Recommended this
become a standard step told to clients upfront (part of the timeline/
price). **Written up as a reusable checklist**, saved at
`Documents/AI Agency/client-onboarding-checklist.txt` (agency-level, not
client-specific, so it's ready for every future client) - covers Gmail,
GA4, Search Console, Meta Business Manager, Google Business Profile, and
DNS/domain, each flagged with what kind of approval it needs.

See also: [[duo-vert/custom-crm-prototype]], [[personal/soccer-coach-website]],
[[duo-vert/season-2027-plan]], [[personal/website-build-playbook]],
[[duo-vert/google-ads-campaign]], [[duo-vert/seo-history]],
**Agency's own website/landing page - backlogged (2026-09-01):** Emile
floated eventually needing his own site/landing page to advertise the
agency and run ads. Same sequencing logic applied as the ops-CRM idea
above: recommended NOT building this yet - it would launch as an empty
portfolio with no real project to show. Once the soccer coach's site is
delivered, that becomes the first real case study and the landing page
has something to actually sell. Filed as a real backlog item, not
dismissed - just gated on finishing the first client project first.

See also: [[duo-vert/custom-crm-prototype]], [[personal/soccer-coach-website]],
[[duo-vert/season-2027-plan]], [[personal/website-build-playbook]],
[[duo-vert/google-ads-campaign]], [[duo-vert/seo-history]],
[[duo-vert/backlink-campaign]], [[feedback/reasoning-and-pushback]],
[[personal/motivation-and-mission]], [[feedback/no-paid-setup-before-ready-to-use]],
[[feedback/proactive-opinions-and-next-steps]].

**Real referral channel surfaced (2026-09-01) - see
[[personal/stephane-referral-pipeline]] for full detail.** A business
consulting agency contact (Stephane, introduced by Emile's dad) refers
web work out to Emile instead of building sites in-house. One real client
(real estate business, content already prepared) is close - info expected
by 2026-09-04 - plus two "probable" additional leads via Stephane's
brother. This is the client-acquisition answer the "finding clients is
untested" risk flagged earlier in this file was waiting on - a warm
referral channel through a real intermediary, not cold outreach or a
marketplace.

**Vibiz discovered as an already-connected tool (2026-09-02):** prompted
by Emile asking how to structure a full-service agency like his friend's
(apexmarketingsolutionssd.com - paid ads, landing pages/funnels, CRM/
automation, content/creative, marketing strategy), a check of connected
MCP servers surfaced Vibiz - a marketing platform already authorized under
Emile's Duo Vert Gmail (`duo.vert.gatineau@gmail.com`), workspace
"Duo Vert's portfolio". Confirmed via `vibiz_whoami` (real, not assumed).
It covers most of that exact 5-pillar stack: ad campaign creation/
management across Meta, Google, TikTok, LinkedIn, Pinterest, and Twitter;
funnel and qualification-funnel generation; lead finding/enrichment; ICP
and offer generation; ad creative/UGC video/product-shot generation;
social scheduling and inbox/comment automation; competitor ad research.
**Caveat: confirmed empty, not proof of anything yet** -
`vibiz_onboarding_status` check showed `accessibleVibizCount: 0`, no
business workspace has actually been set up inside it. Real tool, zero
track record.

**Recommendation given:** same "test on Duo Vert first" pattern as
everything else in this file - set up a Vibiz workspace for Duo Vert,
generate an ICP/offer/funnel and a small real ad campaign, and use that
as the judgment-building rep before pitching ads/funnels to a stranger.
Directly revives the shelved [[duo-vert/google-ads-campaign]] idea with
better tooling than the manual setup originally planned.

**Answer given to "do I need more knowledge, or just you and time":**
split into two kinds of work, not one answer. Mechanical execution
(building the funnel, generating creatives, launching the campaign) is
now largely tooling-assisted via Vibiz + Claude Code - closer to "me and
time" than to studying marketing for a year. Judgment (is this offer
good, is this CPL good or bad for the industry, kill this ad or not) is
the part that still requires real expertise Emile doesn't have yet and
that carries real client cost if wrong - same category of risk as the
"ad recommendations" concern already logged above in this file, not a
new risk, just a new surface for the same one.

**Structural advice given:** don't launch as a full 5-pillar "we do
everything" agency on day one - keep the existing website -> CRM ->
ads/funnels/creative -> retainer ladder, and let "full rebuild" become
the pitch to lead with only once 1-3 have a real track record, not
before.

**Correction from Emile, adopted (2026-09-02):** the "own it, don't rent
it" logic that justified building a custom CRM doesn't automatically
apply to Vibiz, and Emile's own reasoning for why is correct. The CRM is
the client's product - they use it, so it has to fit their business,
which is why it's worth building custom per client (or per-vertical
template, per his own refinement below). Vibiz-type tooling (ads,
funnels, creative, social, lead-gen) isn't client-facing - it's Emile's
own production equipment, the same way a real agency uses Meta Ads
Manager or Adobe Premiere without owning those either. Differentiation
lives in strategy/output, not in owning the ad-management pipeline. This
reverses the earlier framing in this file that treated Vibiz like the
GoHighLevel-for-CRM situation.

**Vibiz real pricing confirmed (2026-09-02, via vibiz.ai/pricing
directly, not a third-party review site):** three tiers, all "per member/
month," credits non-accumulating (don't roll over): Plus $24 (100
credits/mo, 2 workspaces, 3 team members), Pro $40 (250 credits/mo, 5
workspaces, 10 team members), **Ultra $64 (500 credits/mo, unlimited
workspaces, unlimited team members)**. Plus a 2% service fee on ad spend
across all tiers. Practical read: Ultra at $64/month flat covers Emile's
entire agency, one workspace per client, not a per-client cost - cheap
enough that there's no real cost argument for replicating this via raw
API integration instead. Real constraint to watch as client count grows:
the 500 credit/month cap is shared across all workspaces and doesn't
accumulate, could become a real limit with several active clients
generating creative/copy regularly - not a problem yet, worth monitoring
if/when this gets used for real. The 2% ad-spend fee needs to be a real
line in client pricing (absorbed or passed through), not left implicit.

**Integration point clarified (2026-09-02), asked directly ("would a
Claude-built CRM/website be compatible with Vibiz"):** the two systems
don't need to talk to each other constantly - the real question is only
where a captured lead lives. Two options: (1) point Vibiz-driven ad
traffic at Emile's own custom website/landing page (built in Claude
Code), whose form submits directly into the CRM he built, same as Duo
Vert's own site today - Vibiz never touches lead data, purely the ad
engine, CRM stays the single source of truth. **Recommended as the
default.** (2) use Vibiz's own generated landing pages instead, where the
lead sits in Vibiz's own leads board and would need syncing into the CRM
afterward - **not yet confirmed whether Vibiz exposes an API/webhook for
that sync**, flagged as unverified rather than assumed, so this path is
not something to rely on without checking first.

**Real agency-industry research done (2026-09-02), not guessed - Emile
asked directly how agencies normally structure client reporting/ad
ownership and how AI changes it, since he was confused whether it made
sense for his CRM to show ad performance to a client when he's the one
operating the ads.** Confirmed via web search, not assumed:

- **Client dashboard confusion resolved:** standard industry practice,
  not a contradiction - the agency operates the ads, the client gets a
  read-only branded reporting portal (spend, leads, results) without ad
  account login access. A whole software category exists for exactly this
  (AgencyDashboard, ClicData, Cometly). Emile's CRM already being the
  reporting surface (the digest/trends feature from
  [[duo-vert/custom-crm-prototype]]) puts him ahead of agencies that pay
  for separate reporting software.
- **Ad account ownership - two real models, recommendation given:**
  agency-owned (more control, client locked in if they leave) vs.
  client-owned with agency admin access (client keeps control, standard
  trust-building direction the industry is moving). **Recommended:
  client-owned, Emile-administered** - same logic already applied to
  domain ownership (client owns domain, Emile operates hosting).
- **Real pricing benchmarks found:** ad management is priced separately
  from CRM/hosting retainers because it's ongoing labor, not a one-time
  build. Two standard models: flat retainer ($1,500-$10,000+/mo for real
  active management) or percentage of ad spend (15-20% under $5k/mo
  spend, sliding to 8-12% above $100k/mo spend) - roughly a third of
  agencies use each model (flat/percent/hybrid). Illustrative first-client
  number given: $1,000-2,000/mo ad spend at 15-20% management fee =
  ~$150-400/mo, **a separate line from the existing $150-300/mo CRM+site
  retainer**, not folded into it.
- **AI-era context (2026), treated as real current pattern not
  hype:** roles that used to require a strategist, media buyer,
  copywriter, designer, and account manager are genuinely being run by
  one person plus an AI stack (Claude Code equivalent) as of this date -
  confirms the underlying premise of this whole idea is not wishful, real
  solo operators are doing this now, though the human judgment/client-
  relationship parts stay Emile's regardless.
- **Full fee structure given, replacing the earlier single-retainer
  framing:** build fee (one-time) + CRM/hosting retainer ($150-300/mo) +
  separate ad management fee (flat or %-of-spend) once ads actually run -
  three distinct lines, not one bundled number.

Sources: [Agency Client Reporting
Dashboards](https://www.cometly.com/post/agency-client-reporting-dashboard),
[Marketing Agency Org
Chart](https://agencydashboard.io/blog/marketing-agency-org-chart), [Ad
Agency Pricing Models](https://www.get-ryze.ai/blog/ad-agency-pricing-models-flat-fee-percentage),
[Marketing Agency Retainer
Pricing](https://clicksgeek.com/marketing-agency-retainer-pricing/),
[Marketing Agency Cost
2026](https://www.darkroomagency.com/observatory/marketing-agency-cost-2026-pricing-by-service).

**Packaging structure researched and resolved (2026-09-02) - Emile asked
directly how to package services when different clients want different
combinations (website only, CRM only, or everything), initially guessing
a forced base/medium/premium ladder, explicitly asked to be grounded in
real agency practice rather than his own guess.** Confirmed via web
search: the dominant real pattern is not a single forced tier ladder -
it's "core services each priced standalone, plus a small number of
pre-built bundles at a discount for common combinations," described in
one source as "core + menu of modules." 85% of agencies do use tiered
packages (faster to sell than a blank proposal), but the tiers sit on top
of standalone per-service pricing, they don't replace it.

**Resulting structure recommended for Emile specifically:**
- Standalone, each independently priced: Website only ($600-2,500+ +
  $15-30/mo hosting), CRM only for a client with an existing site (build
  fee scaled to complexity + $150-300/mo retainer), Ads management only
  for a client with existing site/CRM (flat or 15-20%-of-spend, no build
  fee - a real standalone "media buying" service in the industry).
- Bundles at a discount for combinations: Website + CRM ("digital
  foundation," the $8,000-12,000 Duo-Vert-scale number already priced,
  lower for a simpler first client), and Full rebuild (website + CRM +
  ads management + reporting, the top "do everything" pitch, sold once a
  track record exists per the earlier ladder logic in this file).

Sources: [Agency Pricing in 2026](https://www.swydo.com/blog/agency-pricing/),
[Agency Pricing Models: The Complete Guide for 2026](https://thestacc.com/blog/agency-pricing-models/),
[What is a Modular Pricing Model?](https://www.getmonetizely.com/articles/what-is-a-modular-pricing-model-a-guide-for-saas-executives).

**GoHighLevel real pricing checked (2026-09-02) - Emile had assumed
~$90/mo flat, asked directly whether $1,000 build + $50/mo retainer was
viable against that.** Confirmed via web search, not assumed: GHL is
$97/$297/$497 per month across its three tiers, and SMS/calls/email/AI
usage bill separately on top at another $20-150/mo - real all-in cost is
usually $120-650/mo, meaningfully higher than the $90 Emile remembered.
**Pushback given on his own $50/mo figure:** flagged as underpricing
against both the market (GHL customers already accept $97+/mo, so
$150-300/mo is still a discount, not a hard sell) and against his own
earlier-established number - the $150-300/mo CRM retainer already set in
this file, not $50. Math shown: $50/mo x 10 clients = $500/mo total
recurring, not enough to matter; $150-300/mo x 10 clients = $1,500-
3,000/mo, the number that makes the recurring layer worth building.
Also flagged the $1,000 CRM build figure as likely too low given the real
engineering scope, closer to the $2,000-5,000 range already scoped for a
simpler-than-Duo-Vert build. **Recommendation: hold the line at
$150-300/mo retainer and $2,000-5,000+ build fee, not $50/$1,000.**

**Client acquisition confirmed via research (2026-09-02), validates
existing approach rather than changing it:** personal network/referrals
confirmed as the fastest real path (matches the existing
[[personal/stephane-referral-pipeline]] channel exactly). Structured
cold outreach given as the real secondary channel once warm network isn't
enough: pick one specific niche, build a list of 100 real businesses in
it, write one personalized email template with a clear per-business
variable - not generic mass outreach. Local in-person (Chamber of
Commerce, driving a target radius, a free quick audit as the icebreaker)
given as a third channel for volume beyond the warm network.

Sources: [GoHighLevel Pricing 2026](https://netpartners.marketing/gohighlevel-pricing-plans-explained-features-value-cost-comparison-2026/),
[GoHighLevel Pricing 2026: Real Add-On Costs](https://automatethejourney.com/blog/gohighlevel-pricing-plans-2026),
[Cold Outreach for Local Business Agencies](https://www.systemifyautomation.com/blog/why-cold-outreach-is-the-most-important-growth-channel-for-agencies-serving-local-businesses).

**Ongoing client-acquisition research done (2026-09-02), extends the
first-client research above to how agencies sustain client flow long
term.** Confirmed via web search: referral partnerships are only ~10% of
a typical agency's pipeline but generate 31% of revenue, the single
highest-value source per lead, and convert 3x higher than cold outreach -
directly validates [[personal/stephane-referral-pipeline]] as Emile's
best channel, not just a lucky early break. Case studies convert 23%
higher than a generic portfolio, backing the existing "build 1-2 real
sites, then use them as proof" sequencing already planned. Content
marketing (a page/post that ranks and also demonstrates the actual skill,
e.g. a real result written up) works as a lead magnet and case study at
once - flagged as a channel Emile is unusually well-positioned for
already, since he already does real SEO work on Duo Vert. Winning
agencies combine 3-5 acquisition channels at once, not one - for Emile
that's realistically referrals (working now) + case studies (once the
real estate site ships) + his own SEO'd agency site (still backlogged on
finishing a first project, per earlier entry in this file).

Sources: [How Marketing Agencies Get Clients: Every Channel
Ranked](https://prometheanresearch.com/how-marketing-agencies-get-clients/),
[How To Get Marketing Clients in 2026: 18
Strategies](https://assembly.com/blog/how-to-get-clients-for-marketing-agency).

**Self-doubt about untested skills resurfaced (2026-09-02), same
underlying concern as the ad-recommendations judgment risk logged
earlier in this file, not a new worry:** Emile explicitly separated what's
proven (CRM, website - both real via Duo Vert) from what isn't (ads,
funnels, creative - never actually tested), and asked why a business
owner would pay him over doing ads themselves. Also said, notably, "I'm
sure with you it's gonna be easy" - flagged and pushed back on directly,
not accepted at face value: Claude can help execute and catch mechanical
mistakes but cannot supply the judgment itself (is this ad good, is this
targeting sane) - that only comes from Emile's own reps, which is the
actual reason the existing "test ads on Duo Vert's own account before
selling to a stranger" plan matters, not busywork.

**Real economics of paying for ad management, researched not asserted:**
small business owners average ~20 hours/week on marketing when self-
managed - the real value proposition isn't "smarter than the owner,"
it's time/opportunity cost (their highest-value hour is running their
actual trade) plus breadth (nobody is genuinely expert across SEO, ads,
design, and CRM at once). Source: [Is It Worth It to Hire an Advertising
Agency?](https://rooksagency.com/hiring-advertising-agency/).

**Recurring personal time breakdown given (2026-09-02), asked directly
("where would I actually spend time each month, since you can execute
most of this yourself"):** split into four real categories rather than
one "check up occasionally" bucket:
- Near-zero recurring time: website edits, CRM hosting/maintenance,
  digest reports (already automated per
  [[duo-vert/custom-crm-prototype]] Round 9-10), social scheduling -
  Claude executes, Emile is notified only if something breaks.
- Light but real, every time: a quick review before anything client-
  facing ships (skim the digest, brand-check new ad creative, tone-check
  GBP replies) - minutes per client, but must actually happen, not be
  skipped.
- Genuinely needs Emile's own judgment, not just approval: ads -
  reviewing performance and deciding to kill/scale/adjust a campaign is
  the real recurring judgment work, same category already flagged
  repeatedly as needing his own track record first, not something to
  delegate to approval-only.
- Never offloadable at any scale: the actual client relationship -
  answering questions, handling problems, selling the next client.

**Scaling point flagged:** this is light per client but linear - 10
clients means 10 of these small weekly/monthly checks, not one. Directly
ties to the already-backlogged "agency ops CRM to track what's due per
client" idea earlier in this file - confirmed as the real answer to
managing this at scale, just not needed yet at 1-2 clients.

**Emile's own concrete 6-month revenue target, stated 2026-09-02:** 3
bundle clients ($6,000 build fee each + $200-300/mo retainer each)
within 6 months = ~$18,000 in build fees plus ~$700/mo recurring by the
end of that window. Confirmed as a realistic, not inflated, target ($6k
for ~20-30 hours works out to ~$200-240/hour effective, consistent with
what Claude Code actually compresses). **Real gap flagged, not just
encouragement:** his current 3 real leads (see
[[personal/stephane-referral-pipeline]]) are website-only conversations
right now, not $6k bundle deals - hitting this target runs through
actually upselling CRM once each site ships (the existing website-first-
then-CRM ladder), not assuming the bundle price applies to leads already
in motion. Longer-term revenue escalation floated by Emile in the same
message ($5k -> $10k -> $20k -> $50k/month) treated as directional vision,
not something requiring its own plan yet - the real near-term target is
the 6-month/3-client number above.

**Resulting recommended structure (2026-09-02):** one shared Vibiz
account (Ultra tier) handles every client's ads/funnels/creative/social/
lead-gen, one workspace per client. The CRM stays custom-built per
client on Emile's own codebase, with Emile's own refinement of doing this
by industry-vertical template (e.g. a landscaping/outdoor-work template,
a separate ecommerce template) rather than either fully bespoke each time
or one rigid CRM for everyone - a genuinely good middle ground on the
existing "fork per client" pattern. **Recommendation: use Vibiz rather
than building a replica** - the engineering cost of replicating Meta/
Google/TikTok/LinkedIn ad APIs plus a funnel builder plus creative
generation would be real (weeks to months), for something that isn't the
differentiator anyway, at a price point ($64/mo flat, unlimited clients)
where there's no cost pressure to justify building it in-house.
