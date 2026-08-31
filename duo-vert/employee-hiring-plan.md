---
name: duo-vert-employee-hiring-plan
description: "Planning for hiring door-to-door sales reps + manual labor cleaners for the 2027 season, compensation structure design"
metadata:
  type: project
  modified: 2026-08-28
---

**Candidate tracking list — created 2026-08-28, went through two format iterations
same day before landing on a real spreadsheet.** Lives at
`Documents/Duo Vert/Admin/Hiring/Duo Vert - Liste de candidats.xlsx` — a formatted Excel
sheet (tab "Candidats": Nom / Poste / Statut / Notes, colored dropdown for Statut,
example row marked "Ex:") for names Emile is considering for sales rep or cleaner
roles, matching the "hire people we personally know first" approach in the sales-crew
decision below. Emile opens/edits it in Numbers or by importing to Google Sheets, same
pattern as [[duo-vert/sheets-tracking|the Depenses sheet]] — built locally with
openpyxl, handed to him, never driven live in a browser (see
[[feedback/build-locally-not-live-browser]]). Emile will ask Claude to add/update rows
as names come up in conversation.

**Format history, worth remembering before building another simple doc for Emile:**
first built as `.md` with a markdown table — displayed as raw markdown/code since Emile
wasn't viewing it in a markdown-aware app. Rebuilt as plain `.txt` — Emile rejected that
too, said he wanted "Google Sheets, Docs, Word, something with actual formatting/graphs,"
not a bare text file. Landed on `.xlsx` as the actual right answer. **Takeaway:** for a
structured list/tracker (columns, statuses, anything he'll scan repeatedly), default
straight to a real spreadsheet (xlsx, built locally per the skill/pattern above) — don't
route through markdown or plain text first. Reserve plain `.txt` for genuinely
unstructured prose notes, not row-based data.

**Started 2026-08-25.** Emile and Beckett decided (after a light 2026 season — didn't
work much: June to ~July 10, then not again until ~Aug 10) that for their third
consecutive summer running Duo Vert (2027 season), they want to hire employees rather
than stay a 2-person shop. Two roles planned:

1. **Door-to-door sales reps** — canvass, hand out flyers, pitch Duo Vert's services,
   paid commission per client they land. Emile and Beckett are fine with reps working
   in pairs (confidence/performance boost), but want to avoid paying double commission
   (e.g. 15%+15%=30%) when two reps work the same door.
2. **Manual labor cleaners** — do the actual restoration/cleaning work. Unsure whether
   to pay hourly (risk: workers go slow) or commission/% of job (risk: workers rush and
   do sloppy work). Also want a quality bonus (tied to 5-star reviews) and some kind of
   penalty/pay-cut if a job comes back bad or a client complains.

Timeline: ~2 more weeks of active work left in the 2026 season (to mid-September), then
Emile has mid-September through May to design the system, recruit, and prepare before
the 2027 season starts. Explicitly wants to maximize prep time to be efficient and
profitable from day one next summer.

## Recommendations given 2026-08-25

**Sequencing:** the 2026 retro (see [[duo-vert/revenue-growth-plan]]) found the real
bottleneck this season was demand, not capacity — one client most of the summer until
Meta ads started working. Door-to-door sales is actually a demand-generation fix. Labor
hires only pay off once sales is producing real volume — recommended bringing sales on
first (or a few weeks ahead of labor) rather than staffing both up cold in May, to avoid
paying idle cleaners. **Emile's response (2026-08-25):** he's confident they can find
enough jobs for cleaners to be genuinely useful next season — pushing back on the
staggered-hiring caution above. Not fully resolved; worth revisiting once sales-channel
performance is more concrete during 2027 planning.

**Full 2026 season retro numbers (2026-08-25, final tally):** only **5 jobs total** all
summer. Website produced essentially zero leads. Door-to-door effort was only **~12
hours total** for the entire season, a token effort, not a real test. This is the
baseline being compared against for 2027 planning.

**Note on a different public-facing figure (2026-08-27):** Emile's LinkedIn profile
states "20+ clients in 2026," and he confirmed 2026-08-27 to keep using that number
there. Not treated as a contradiction of the 5-jobs internal retro figure above —
likely a different counting scope (e.g. cumulative clients touched/quoted since the
business started, or since Mar 2025, rather than jobs completed in the narrow summer
2026 season window). Use the 5-jobs figure for internal planning/capacity math, and
don't assume the two numbers need to reconcile without asking Emile directly if it
matters for a specific decision.

**Demand-forecasting approach — DECIDED 2026-08-25: no target multiplier, test and
track instead.** Emile asked whether hiring real sales reps could roughly double+ job
volume next season. Researched general D2D industry benchmarks (50-70 doors/day for a
top rep, 15-20 doors/hour suburban, 2-5% overall D2D conversion, 20-35% close rate on
home-services full pitches) but concluded these don't transfer cleanly to Duo Vert
specifically — the benchmarks span low-commitment/subscription D2D (pest control,
alarms) vs. Duo Vert's $600-4,000 one-time service requiring a proper site visit and
quote, and naively applying them produces implausibly high numbers (1-3+ sales/rep/day).
With only 12 hours of real-world door-to-door data ever collected, there's no basis for
a confident Duo Vert-specific conversion rate yet. **Recommended plan instead:** treat
the first 2-3 weeks of real door-to-door effort in 2027 as a live test, track doors
knocked → quote-visits booked → jobs closed, and use that real conversion data (not an
industry average) to plan cleaner staffing for the rest of the season. Directionally,
"more than double" 2026's 5 jobs is very likely a floor given the effort jump from 12
hours to a real season of work, but the true ceiling is unknown until tested. Capacity
to deliver higher volume is less of a concern than in 2026 specifically because cleaners
are now hired help, not just Emile/Beckett doing 100% of labor themselves.

**Demand vs. capacity projection worked 2026-08-25, June 20 – Aug 20 window (~61
days/8.7 weeks):** Emile's conservative planning assumptions — sales team (2 reps
combined) bringing in 3 clients/week, Facebook ads at 2 leads/day converting 1-in-5,
website at 1 lead per 2-3 days (converting 1-in-5, assumed same rate as Facebook, not
explicitly given by Emile) — sum to **~55 projected jobs** over that window (~26 from
sales, ~24 from ads, ~5 from website). Checked against cleaner capacity using the
job-time data above (~6 hr wash day + ~2.5 hr sand day per job): **one 2-person
cleaning crew tops out around ~43-44 jobs in the same window** (roughly 5 new jobs
startable per week). **Real finding: at these conservative assumptions, demand would
outstrip a single 2-cleaner crew by ~11-12 jobs.** This revises the earlier "capacity
is less of a concern now" note above — it's only true if cleaner headcount scales with
demand. Three options flagged for Emile to decide between before 2027: (1) hire a
second 2-person cleaning crew (4 cleaners total, not 2), (2) Emile/Beckett personally
absorb overflow jobs beyond crew capacity, (3) deliberately throttle lead volume (ease
off sales-rep push or pause ads) once pipeline outpaces delivery capacity. Not yet
decided.

**Sales commission structure:** commission per deal, not per person — a team (solo or
paired) splits one commission pool (e.g. 15% of the labor-only quoted price, split
50/50 or by internal agreement if two reps worked the door together). Total payout per
deal never exceeds the set rate regardless of team size. Since Duo Vert's quoted price
is already labor-only (materials billed separately at cost, no markup — see
[[duo-vert/company]]), commission on the full quote is commission on margin-bearing
revenue, no adjustment needed. Still needs: actual rate modeled against doors-per-day
and expected close rate before locking a number, since $600-4,000 jobs at 15% is only
$90-600/sale and this is the rep's whole income (no base).

**Labor pay structure — DECIDED 2026-08-25: % of job price (commission), not flat
piece-rate.** Both failure modes Emile originally named (hourly → slow work, commission
→ rushed sloppy work) are solved by the same mechanism regardless of flat-rate vs. %:
pay not tied to hours, plus payout gated on passing inspection rather than on speed —
that part is unaffected by this choice. Emile settled on % over flat-rate-by-scope for
three reasons specific to Duo Vert, given in response to the initial flat-rate
recommendation:
1. The front+back discount (15% off, see [[duo-vert/soumission-template]]) is a fixed,
   known company policy applied ~95% of the time, not ad-hoc per-client negotiation —
   so the "pay varies with Emile's invisible sales decisions" transparency risk doesn't
   really apply here.
2. Job scope varies beyond sqft (leveling sometimes needed, sealant sometimes needed)
   and price already encodes that variation — % of price automatically scales cleaner
   pay with real added work, where a flat sqft-bracket rate would need constant manual
   patching to capture the same thing.
3. Sqft measurements are estimates, never exact — tying pay to a separately-calculated
   sqft figure would create a second disputable number in addition to whatever the
   client agreed to. Tying pay to the agreed sale price avoids that second dispute
   surface entirely.
Same team-split rule as sales applies here too: if two cleaners work one job together
(plausible, since Emile/Beckett currently always work jobs as a pair), the commission
splits between them, doesn't pay in full to each. Review-linked bonus (5-star trigger)
sits on top as the extra performance incentive Emile already wanted. Before locking an
actual %, model a typical job with sales commission % + cleaner commission % +
materials-at-cost all stacked against price, to confirm real margin left over for
Emile/Beckett.

**Quality control — legal flag:** Emile's instinct to dock pay for bad work (e.g. take
back half) is risky under Quebec's Loi sur les normes du travail, which restricts
deductions from already-earned wages. Cleaner framing: final piece-rate payment is
contingent on passing an inspection checklist (Emile/Beckett check the work, same as
verifying a contractor) — a failed job just doesn't trigger payout until fixed, rather
than clawing back money already paid. Same practical effect, no wage-deduction exposure.
Needs a written checklist of what "passing" means to avoid it being a judgment call.

**Draft numbers being modeled 2026-08-25 (not locked, Emile said "not fully sure"):**
worked a $2,000 labor-only job example at 15% sales commission (one pool, split by the
sales team) + 15% commission **per cleaner** (not a shared pool, mirrors sales in rate
but not in split-logic) = $300 sales + $300/cleaner. On a 2-cleaner job that's $900 paid
out (45%), $1,100 (55%) left for Duo Vert before overhead. Two open questions raised
against this draft: (1) whether paying each cleaner their own full 15% (vs. splitting
one pool like sales does) is intentional — arguably justified since an extra cleaner
adds real labor/output unlike an extra sales rep on the same door, but not yet confirmed
as deliberate; (2) whether cleaners will typically work jobs solo or always paired like
Emile/Beckett currently do, since that swings real per-job margin between ~70% (solo)
and ~55% (paired).

**WORKING NUMBERS LOCKED 2026-08-25: sales 22%, cleaners 12% each.** Landed here after
market research + job-time math (below). Sales 22% sits inside the 15-30% pure
commission-only norm. Cleaners 12% each nets ~$25/hr solo or ~$38-40/hr per cleaner if
two work a job together with their own washers (see hourly-rate math below) — both above
Quebec market wage references, consistent with Emile's "better than minimum wage,
better than other jobs" goal. On a $2,000 job this is 34% paid out (solo cleaner) to
46% (paired cleaners), leaving 66%/54% for Duo Vert before overhead — not yet checked
against real overhead numbers, revisit once those or a season of real data exist.
Team splits like 15%/5% closer-vs-support for sales pairs remain the agreed approach
from earlier, that detail is unaffected by the rate bump to 22%.

**Market research done 2026-08-25 (web search, see chat for full source list):**
- Sales: pure commission-only field/door-to-door (no base salary) industry norm is
  **15-30%**. Roofing/construction commission-only runs up to ~15% of revenue; landscape
  salespeople specifically average ~8% but that's almost always salary+commission, not
  comparable to Duo Vert's no-base model. Conclusion: **20% commission-only is well
  within normal range**, not generous, given zero base/downside protection for the rep.
  15%/5% closer-vs-support split is standard industry practice.
- Cleaners: **no real "% of job price" benchmark exists for manual labor crews** — the
  landscaping/paver industry almost universally pays hourly or flat-per-estimated-hours,
  not commission. Reference points instead: Quebec landscaper median wage **$17.89/hr**
  (~$41K/yr average), Quebec minimum wage **$16.60/hr** (as of 2026-05-01), US
  landscape-construction crew with experience **$27-35/hr** (up to $90K/yr with
  bonuses/profit-sharing). Right way to pick the cleaner % isn't "match an industry %,"
  it's back into an effective $/hour from the % and check it clears that $17-35/hr band,
  consistent with Emile's stated goal of paying noticeably better than minimum wage.
  **Blocked on:** typical person-hours to complete a $2,000 job with 2 cleaners working
  together — needed to convert any % into an effective hourly rate and actually finish
  this calculation. **Equipment note (2026-08-25):** each hired cleaner will get their
  own pressure washer (Duo Vert-provided) — currently Emile/Beckett share just one
  machine between the two of them, which Emile says adds real time to jobs. So the
  job-duration baseline for time estimates should account for 1-per-cleaner equipment,
  not the current 1-shared-washer setup; expect hired-cleaner job times to run faster
  than Emile/Beckett's own historical times for the same scope.
  $2,000 at ~$2.50/sq ft ≈ 800 sq ft, used as the reference job size for this time
  estimate (Emile first floated $2.90/sq ft, then revised to $2.50/sq ft — this is a
  working reference rate for this calculation, not confirmed as Duo Vert's actual
  standard pricing rate; see [[duo-vert/company]] for the ~$3/sq ft market-benchmark
  figure already on file).

**Job time breakdown, ~800 sq ft / $2,000 job (Emile's rough estimates, 2026-08-25,
current 1-washer/1-broom/1-compactor equipment baseline):** pressure washing 5-6 hrs
(day 1) → cleanup of overspray/debris on windows/doors/grass +0.5 hrs (same day) →
sanding with 1 broom + compactor 2.5 hrs (separate day, weather-dependent) → sealant
application 1 hr (~1 month later, separate visit). **Total ≈ 9.5 hrs across 3 site
visits.** Not yet confirmed whether the 5-6 hr wash figure is one person working alone
or elapsed time with 2 people present sharing the single washer — matters for the
per-person hourly math below and still needs confirming.

**Effective hourly rate math from the above (2026-08-25):**
- Solo cleaner doing all 9.5 hrs alone: 10% → $200 (~$21/hr), 12% → $240 (~$25/hr),
  15% → $300 (~$32/hr). 12% lands just under the $27-35/hr experienced-crew reference
  band, 15% lands in the middle of it.
- Two cleaners each with own washer (per the pressure-washer-per-cleaner equipment
  plan above), assuming the wash step roughly halves (~2.75 hrs each) but
  sanding/sealant stay close to full time since that equipment isn't duplicated: each
  cleaner's individual time ≈ 6-6.5 hrs, while each still collects their own FULL %
  (not split, per the earlier per-cleaner-not-pooled design decision) → 12% each →
  ~$38/hr each, 15% each → ~$48/hr each. **Flagged as a real design tension:** because
  cleaner pay is per-person-not-pooled, buying better equipment to speed up the job
  raises each cleaner's effective hourly rate rather than lowering Duo Vert's cost —
  opposite of the usual effect of an efficiency investment. Worth deciding on purpose,
  not landing on by accident.

**Physical crew requirements per step (Emile, 2026-08-25):** wash+cleanup genuinely
needs 2 people (longest, hardest step). Sanding genuinely needs 2 people too — the
heavy plate compactor is "practically impossible to lift on your own." Sealant only
needs 1 person — having 2 there is a waste of time. This resolves most of the earlier
"is the 5-6hr wash figure solo or shared" ambiguity: in practice wash and sanding will
almost always be 2-person steps, solo work will mostly only happen on the sealant trip
(~1 month later, separate visit).

**Per-step commission split — DESIGNED 2026-08-25, to handle solo vs. paired work
fairly without double-paying or penalizing a solo cleaner:** break the locked 24% total
cleaner pool (12% × 2) into per-step pools weighted by time: wash+cleanup 16% (of job
price), sanding 6%, sealant 2% (16+6+2=24). Rule: whoever works a given step splits
that step's pool; whoever does a step alone keeps the whole thing. Reduces to the
existing 12%-each baseline when both cleaners do everything together (8+3+1=12 each).
Realistic case given the crew requirements above: both split wash+sand (8%+3%=11%
each), whoever does the solo sealant trip picks up the extra 2% alone (11%+2%=13% for
that person). Still open: whether the same cleaner always handles the sealant
follow-up or it rotates between the two — determines who collects that extra 2% each
job.

**5-star review bonus — DECIDED 2026-08-25: flat $50 EACH cleaner** (not split — if
both cleaners worked the job, that's $50 + $50 = $100 total per 5-star review), not
paid to sales. Flat rather than %-of-job since review likelihood doesn't scale with
job size. Exception: if one cleaner clearly carried the job solo, that's a case-by-case
call, not a fixed rule.

**Example payouts worked out 2026-08-25, two scenarios** (24% cleaner pool split by
the step-split above between the 2 cleaners on the job, materials excluded
throughout):
- **Scenario A — a hired sales rep brought in the lead:** 22% sales / 24% cleaners /
  54% owners (Emile+Beckett combined, assumed 50-50 split between them). $1,000 job →
  sales $220, cleaners $240 total, owners $540. $2,000 job → sales $440, cleaners $480,
  owners $1,080. $4,000 job → sales $880, cleaners $960, owners $2,160.
- **Scenario B — Emile or Beckett sourced the lead themselves** (ads, referral, no
  sales rep involved): no sales commission paid, that 22% stays with the owners
  instead → 0% sales / 24% cleaners / 76% owners. $1,000 job → cleaners $240, owners
  $760. $2,000 job → cleaners $480, owners $1,520. $4,000 job → cleaners $960, owners
  $3,040. Cleaner pay is identical in both scenarios since their work doesn't change
  based on lead source — only the owners' take shifts depending on whether the 22%
  went out to a rep or stayed in-house.

**Conservative/confident-floor scenario worked 2026-08-25, same June 20 – Aug 20 window:**
Emile deliberately lowered the earlier assumptions to find a number he's actually
confident Duo Vert can hit — Facebook ads down to 1 lead/day (from 2), sales team down
to 2 clients/week (from 3), 1-in-5 conversion kept the same throughout, website
unchanged, every job assumed flat $2,000, and cleaners doing 100% of the work (no jobs
done personally by Emile/Beckett). Result: **~34 jobs total** (~12 from ads, ~17 from
sales, ~5 from website) → **$68,000 total company revenue** over the window. Split
roughly evenly between sales-sourced jobs (17, using the 22/24/54 split) and
ads/website-sourced jobs (17, using the 0/24/76 split since no rep is involved):
sales reps get $7,480 total, cleaners get $16,320 total, **Emile and Beckett get
$44,200 combined, $22,100 each**, before real overhead. **Important: at these lower,
more-confident numbers, ~34 jobs fits comfortably within a single 2-cleaner crew's
~43-44 job capacity ceiling** (see the demand-vs-capacity finding above) — the earlier
capacity gap disappears at this conservative scenario, making this a genuinely
achievable planning floor rather than an aggressive stretch target.

**Still open / not yet decided:** checking the 22%/24% split against real overhead
numbers once known, whether cleaners get any base/hourly floor while new and still
slow, recruiting channel/where to find people, whether reps/cleaners are employees or
contractors (affects the wage-law question above), written pay agreement docs, the
inspection checklist itself, who handles the solo sealant visit each job, and **the
demand-vs-capacity gap found above (2nd cleaning crew vs. owners absorbing overflow vs.
throttling leads) — the most consequential open decision in this whole plan, since it
determines total headcount to hire, not just pay structure.**

**Sales crew: DECIDED non-negotiable, confirmed with Beckett's dad 2026-08-27**
(corrected 2026-08-28 — this was Beckett's dad, not Emile's own dad; no one else's
input is needed on anything else in this plan). Emile walked Beckett's dad through the
full 2027 marketing/planning vision; Beckett's dad was supportive and liked the
ambition overall, but flagged one specific concern — that a door-to-door sales crew is
risky since reps directly represent the company's public face/reputation. After a long
discussion they landed on: having a sales crew next summer is non-negotiable for Emile,
Beckett's dad agreed, not a fully resolved risk (Emile's own words: "there's never zero
risk") but accepted as the direction. Emile's reasoning for why the
risk is manageable: (1) plan to hire reps from people they personally know first —
friends — specifically so they're people Duo Vert can trust, rather than open/cold
hiring; (2) intend to build a genuinely strong team culture/vibe (see
[[duo-vert/season-2027-plan]] for the culture/vibe goals already being planned) so Duo
Vert becomes a brand people want to work for, which both raises applicant quality and
gives reps a real stake in representing the company well; (3) commission-only pay (see
locked 22% rate above) means zero direct financial risk if a rep underperforms — no
wages paid for jobs not landed. **Initial headcount target: 2 sales reps to start.**
If job volume outpaces what 2 reps can generate, Emile's plan is to either add more
reps or compensate by scaling up manual-labor crew capacity instead — not yet decided
which, explicitly "we'll see when we get there."

See also: [[duo-vert/revenue-growth-plan]], [[duo-vert/company]], [[project-current-todo-list]],
[[duo-vert/season-2027-plan]] (storage unit logistics + culture/vibe goals for these hires)
