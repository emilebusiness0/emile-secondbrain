---
name: duo-vert-sheets-tracking
description: Emile's plan for Duo Vert's Google Sheets back office (leads+clients merged into one sheet, expenses separate) and the lead-tracking automation
metadata:
  type: project
  modified: 2026-08-18
---

Emile wants 3 Google Sheets (tabs, not website pages) to run [[duo-vert/company|Duo Vert]]'s back office:

1. **Leads sheet** — ✅ done, see below.
2. **Expenses sheet** — ✅ done, see below.
3. **Clients/revenue sheet** — 🔄 in progress, being folded into the Leads sheet as a "Clients" tab rather than a separate file (decided 2026-08-18) — see below.

**Why:** no structured tracking previously existed — quote requests only landed as emails, no lead pipeline, no expense or revenue tracking.

## Leads sheet — done and verified working (2026-07-30)

Sheet: "Duo Vert - Suivi Leads". 4 tabs: Pré-soumission, Post-soumission, Perdu, Tableau de bord (live dashboard formulas).

**Automation (Apps Script bound to the sheet):**
- Checking "Soumission envoyée" in Pré → row auto-moves to Post.
- Setting Résultat = "Perdu" in Post → row moves to Perdu, tagged Raison = Refus.
- Both follow-ups checked + 5 days no resolution → daily trigger auto-moves to Perdu, Raison = Sans réponse.
- Lead numbers auto-increment via a script property counter.

**Visual diagram (2026-08-01):** `Excalidraw/lead-webhook-pipeline.excalidraw.md` in this vault — a 4-box diagram (Netlify Form → Proxy → Apps Script → Google Sheet) of the exact flow described below. Open it in Obsidian's Excalidraw view.

**Website form → Sheet pipeline:** duovert.ca's two quote forms — `soumission-gratuite` on `/soumission/`, `soumission-accueil` on the homepage, both Netlify Forms with identical fields (nom, telephone, courriel, adresse, service, message, photo) — send a webhook on submission via Netlify's "Form submission notifications → HTTP POST request" feature.

**⚠️ Critical incompatibility (why a direct webhook doesn't work):** Google Apps Script Web Apps always respond via a 302 redirect to a `script.googleusercontent.com` URL to serve their actual output — unavoidable, baked into the platform. Netlify's webhook sender follows that redirect but preserves POST instead of downgrading to GET (which the redirect target requires), so it always gets **405 Method Not Allowed** back — even though the Apps Script `doPost()` already executed and wrote the row *before* the redirect was issued. After 6 consecutive perceived failures, Netlify **auto-disables the webhook**. This is structural, not a transient bug — pointing the webhook directly at the Apps Script `/exec` URL will always eventually break this way, and no Apps Script code change fixes it.

**Fix — the proxy, full detail:** a small serverless proxy function sits between Netlify and Apps Script. It receives the webhook, forwards it to Google using Node's native `fetch` (which correctly downgrades POST→GET on 302 per spec), and returns a clean `200` to Netlify. Live at **`https://elaborate-gumdrop-199ffb.netlify.app/.netlify/functions/lead-webhook`**, deployed as its own **separate, minimal Netlify site** — NOT part of duovert.ca's actual deploy/repo (`~/Documents/Duo Vert/duovert-site`), so nothing in that repo controls or documents this proxy; it's a standalone Netlify site with its own source. Source: single file `netlify/functions/lead-webhook.js` + a `netlify.toml` with `[functions] directory = "netlify/functions"` (required — Netlify's drag-and-drop manual deploys don't auto-detect a functions folder without it). Both of duovert.ca's form webhooks point at this proxy URL, not at the Apps Script URL directly.

**Resolved 2026-08-01 — proxy source now recovered and stored locally.** Emile downloaded and provided the proxy's source (separate from the `duovert-site` transfer, as expected since it's a standalone Netlify site). `netlify.toml` ([functions] directory = "netlify/functions") + `netlify/functions/lead-webhook.js`. Verified the code matches this doc's description exactly — a Node `fetch` call with `redirect: 'follow'` to the Apps Script exec URL, which correctly downgrades POST→GET on the 302 per spec (unlike Netlify's own webhook sender). **Moved 2026-08-02** from `~/Documents/duovert-lead-proxy` to `~/Documents/Duo Vert/duovert-infra/duovert-lead-proxy` — see [[duo-vert/website-build-overview]]'s site-outage-incident section for why (it sat next to `duovert-site` and got dragged onto the live site's Netlify deploy zone by mistake).

**Deliberately NOT recording the exact Apps Script exec URL here** — this vault's GitHub repo is public, and that URL is a live, unauthenticated POST endpoint into the leads sheet; publishing it would let anyone who finds the repo submit fake leads directly, bypassing the actual website. The real URL is in the `GAS_URL` constant inside `~/Documents/Duo Vert/duovert-infra/duovert-lead-proxy/netlify/functions/lead-webhook.js` on this Mac — check the file directly rather than looking for it here. (This repo isn't currently git-tracked/backed up anywhere else — worth doing at some point the same way `duovert-site` now is, but keep any future repo for it private, not public like this vault.)

The Apps Script has a dedup safeguard (`isDuplicateSubmission`, keyed by Netlify's submission payload) in case Netlify ever retries a delivery — prevents duplicate rows even if a retry occurs. Verified working via a real end-to-end test submission on 2026-07-29 (single row landed, no duplicates).

**Lesson learned:** never drag a folder onto an *existing* Netlify site's deploy area to test something unrelated — did this once and replaced the live duovert.ca site with a placeholder. Recovered via Netlify's Deploys tab (full deploy history is always recoverable), but always use "Add new site → Deploy manually" for anything not meant to replace the live site.

**2026-07-31 update — added "Date visite prévue" column:** Emile wanted the planned quote-visit date visible on Pré-soumission instead of buried in email/texts. Added a new column O ("Date visite prévue", plain date entry, no checkbox) between Follow-up 2 and Soumission envoyée (which shifted from O to P). Updated `Code.gs` accordingly: `PRE_HEADERS` gained the new header, `checkboxes` opt changed from `['M','N','O']` to `['M','N','P']`, and every hardcoded PRE-sheet column reference shifted by one — `handlePreEdit`'s `soumisCol` 15→16, and the `getRange(row,1,1,15)` reads in `moveRowPreToPost`/`moveRowPreToLost` → 16. `addLeadFromForm` and `promptManualLead`'s row-builder arrays each gained one blank/`''` entry before the trailing Soumission `false`. Column insert had a side effect worth remembering: inserting a column left of a checkbox column carries the checkbox *data validation* onto the new column too — had to manually strip validation from the new date column and re-add a checkbox rule to the shifted Soumission column afterward.

## Apps Script gotchas discovered 2026-08-01 (important — will bite again if not remembered)

**`SpreadsheetApp.getUi().alert()` hangs forever (until the 6-min timeout) when a function is run directly from the Apps Script editor's Run button** — the alert has nowhere bound to render since the editor isn't the Sheet's UI context. Any function containing `ui.alert()` or `ui.prompt()` (e.g. `setup()`, which also has a `ui.alert(..., YES_NO)` confirmation) must be triggered from the actual **custom menu inside the Google Sheet** (Duo Vert menu > item), not from the editor. Functions with no UI calls (`removeAllGroupsNow`, `restoreData`, `unhideAllColumns`) are safe to run directly from the editor.

**`setup()` originally called `sheet.clear()` on every tab with zero confirmation** — running it (even by accident, e.g. picking it from the function dropdown instead of the intended one) wiped all lead data instantly. Fixed by adding a `ui.alert(..., YES_NO)` confirmation gate plus an automatic hidden backup (`Backup_[sheet]_[timestamp]`, via `sh.copyTo()`) before any clear. Because of the ui.alert issue above, this confirmation only works when `setup` is run from the Sheet's Duo Vert menu — reinforces why that matters.

**Hidden columns vs. column groups are two different Sheets mechanisms** — don't conflate them. `shiftColumnGroupDepth()` only affects outline/group collapse state (the `groups: [...]` option that used to be in `setup()`, now removed). Simple "Hide columns" (right-click > Hide) is a separate per-column visibility flag, fixed with `sheet.showColumns(startCol, numCols)`. Pré-soumission's Langue/Provenance/Moyen de contact columns were hidden via the *second* mechanism, not the first — wasted a diagnostic round assuming it was groups.

**`onEdit` originally swallowed all errors silently** ("ne pas bloquer les modifications manuelles") — meant `moveRowPreToPost` could fail completely (leaving a lead stuck in Pré-soumission, checkbox still TRUE) with zero visible indication anything went wrong. This is likely what happened when Grace and Perreault's leads didn't move to Post-soumission after checking their boxes, while Emile worked around it by manually copying their data into Post (incompletely, missing Moyen de contact/date fields — that mismatch was the tell that pointed at a silent failure rather than a column-mapping bug). Fixed: `handlePreEdit`/`handlePostEdit` now catch and log the specific move error to `PropertiesService` (key `lastError`) instead of swallowing it via the outer try/catch.

**Current live script state**: centered text alignment added to all data cells; menu item renamed to "⚠️ Reinitialiser tous les onglets (efface tout)" to make the danger obvious. Full corrected script lives in this conversation's history (2026-08-01) — if it needs re-pasting from scratch, regenerate from the lessons above rather than trusting an older cached version.

## Expenses sheet — done (2026-08-02)

Built locally as an XLSX (`~/Documents/Duo Vert/Finances/Duo Vert - Dépenses.xlsx`, generated via a Python/openpyxl script) and handed to Emile to import himself into Google Sheets — per [[feedback/build-locally-not-live-browser]], never drove the live Sheets UI to construct it. He imports fresh copies himself each iteration (File → Import → Replace/new spreadsheet); no live sheet URL is authoritative until he says so.

**Why 2-person split via one shared log, not two sheets:** Emile & Beckett wanted to track who paid for what. Two separate per-person tabs would duplicate categories/totals and require manual sync — recommended and built instead: one shared "Dépenses" log with a "Payé par" dropdown (Emile/Beckett), reconciled on a dashboard.

**Why running balance + bulk settlements, not per-item reimbursement:** per-item reimbursement checkboxes were rejected as too tedious for daily use. Settled on a Splitwise-style pattern instead — dashboard auto-computes net balance owed `(Total Emile − Total Beckett)/2`, adjusted by a separate "Règlements"/"E-Transfer" tab where they log lump-sum paybacks whenever convenient (not per-item).

**Structure — 4 tabs:**
1. **Guide** (first tab) — 3-step plain-language instructions, so Beckett doesn't need to ask Emile how it works.
2. **Dépenses** — Date, Payé par, Catégorie, Description, Montant, Notes. Live totals row (sum + count) at the bottom.
3. **E-Transfer** (renamed from "Règlements" per Emile's preference) — Date, De, À, Montant, Note. Same live totals row.
4. **Tableau de bord** — totals by person, by catégorie (with data bars), by month, and the balance-owed line highlighted in green with a plain-language sentence ("Beckett doit à Emile : $X").

**Catégories (final, after iteration):** Matériaux, Essence/Transport, Équipement, Entretien machinerie, Annonce, Repas/Divers, Autre. Earlier drafts had Assurances/Outils/Marketing-site-web — Emile swapped those out; "Équipement" was added back in a later pass.

**Formatting decisions worth remembering if rebuilding:**
- **Every cell centered** — Emile's explicit instruction after reviewing a draft with mixed alignment (categorical columns centered, description left, amounts right). He wants uniform centering everywhere, no exceptions, including Montant and free-text columns.
- **Native gridlines must stay ON** for the two data-entry tabs (Dépenses, E-Transfer). An earlier draft hid gridlines (`showGridLines = False`) relying only on thin per-cell borders — Emile flagged this as looking "half-finished" past the header row. Fixed by leaving gridlines on for those two tabs (dashboard/guide tabs can keep them off, no complaint there).
- **Category dropdown values need color-coded conditional formatting** (soft fill + colored bold text per category) so the Catégorie column is scannable without reading every cell.
- **Data validation matters for a 2-person shared file**: Payé par/De/À are dropdown-only with error messages (typos would break the SUMIF-based dashboard silently); Montant must be >0; a custom rule blocks "De" = "À" on the same E-Transfer row (would silently corrupt the balance formula).
- Tab colors (green/gold/dark-green/grey) added for at-a-glance navigation between the 4 tabs.

## Clients/revenue sheet — SUPERSEDED same day, folding into Suivi Leads instead (2026-08-18)

First pass: built locally as a standalone XLSX (`~/Documents/Duo Vert/Finances/Duo Vert - Clients Revenu.xlsx`), 3 tabs (Guide/Projets/Tableau de bord), one row per project, deliberately independent from the Leads sheet. Handed to Emile — but on reflection he decided a separate file creates back-and-forth and wants it merged into the existing **"Duo Vert - Suivi Leads"** spreadsheet instead, as a 4th tab alongside Pré-soumission/Post-soumission/Perdu. **The standalone XLSX is now obsolete — don't hand it out again or treat it as current.** Revised plan below.

**Revised architecture (in progress, 2026-08-18):** new **"Clients"** tab in Suivi Leads (`fileId 1dXga1_eRmWTp6JfZJYuHGzEFPwgxYpnNk0HlCJfIaIc`). **Corrected mechanism (Emile caught this, 2026-08-18):** no new checkbox needed — Résultat on Post-soumission already accepts "Gagne" as a value (sitting unused in real data) and the existing `handlePostEdit` already watches Résultat for "Perdu" to trigger the Post→Perdu move. Just extend that same handler with a second branch: Résultat = "Gagne" → move row to Clients, using the identical mechanism already in place for "Perdu", not a separate checkbox/field. Columns on Clients: inherited from Post-soumission (nom/tél/courriel/adresse/langue/provenance/service) + Vendu/géré par (Emile/Beckett), Prix du service, Acompte reçu, Coût matériaux, Solde reçu, Solde dû (calculated), Statut paiement (calculated), Date fin projet, Notes. Dashboard: profit total/par mois/par personne/par service, liste des soldes dus.

**Code.gs delivered (2026-08-19), awaiting Emile's deployment.** He pasted the live script (received via chat — no tool here can read a container-bound Apps Script project's code directly, Drive readers only return sheet data). Full updated Code.gs built, node-syntax-checked, and every existing column index hand-traced through the new version to confirm nothing shifts (new columns are appended at the *end* of each row only, never inserted — this is why the existing automation's hardcoded indices like `resultCol=14`/`soumisCol=16` stay valid). Deployment is two steps: (1) paste the new Code.gs over the old one, (2) run a new **non-destructive** menu item "➕ Ajouter section Clients + compteurs" — it only adds the Clients tab + 2 new counter columns, never calls `.clear()` on Pré/Post/Perdu. The existing "⚠️ Reinitialiser tous les onglets" (which does wipe+backup) was explicitly NOT the deploy path, to protect his 18 live leads.

**Real pre-existing bug found and fixed in the same pass:** `moveRowPreToPost` was silently dropping the Service and Details fields when a lead got promoted from Pré→Post-soumission (never copied into the new row) — unrelated to anything Emile asked for, but it blocked the Clients tab from knowing what service was sold, so fixed as part of this build (Service now carried Pré→Post→Clients, Details folded into Notes).

**Final Clients tab column set (A→T):** # Lead, Nom, Telephone, Courriel, Adresse, Langue, Provenance, Moyen de contact, Service (dropdown), Vendu/géré par (dropdown), Date gagné (auto-stamped), Prix du service, Acompte reçu, Coût matériaux, Solde reçu, Solde dû (calculated), Statut paiement (calculated: Payé au complet/Acompte seulement/Rien reçu), Jours depuis gagné (calculated), Date fin projet, Notes. "Montant soumis" in the source data has inconsistent currency formatting (`2497,99$`, `1,200$` etc.) so it's carried into Notes as reference only, not auto-parsed into Prix du service — Emile enters that cleanly by hand per won project.

**Dashboard extension:** new gold-themed block appended below the existing green lead-funnel block (never clears it) — profit total, matériaux facturés, montant encaissé, total impayé, profit par personne, profit par service, and a live FILTER()-based list of unpaid balances.

**Critical business fact — the payment/materials model (confirmed 2026-08-18, get this right or the dashboard math is wrong):** the price quoted to a client is for **labor/service only** and does NOT include materials. Materials are purchased by Emile/Beckett throughout the job as needed, then billed to the client **at cost, no markup** — a pure pass-through, not a cost that reduces profit. Payment is typically **50% deposit before the job, 50% + materials cost as one final payment after**. So: **Profit = Prix du service only**; Coût matériaux nets to zero on profit (money out equals money billed back in) but still needs its own column since it affects what the client owes and when. This directly contradicts the first XLSX draft's model (Profit = Montant − Coût matériaux) — that formula was wrong and is why the sheet got rebuilt rather than just re-skinned.

**Follow-up reminder — DECIDED (2026-08-18), part of this same build:** Emile rejected a daily email/SMS digest ("too many emails, would blow up our inbox"). Went instead with an in-sheet auto-incrementing day counter: each stage-move function (Pré→Post, Post→Perdu, Post→Clients, and initial add to Pré) stamps a "Date entrée" timestamp on the row; a "Jours dans cette étape" column shows `=TODAY() − Date entrée`, which recalculates automatically whenever the sheet is opened/edited — no trigger needed, so it can't spam anyone. Moving a row to the next stage resets its counter to 0 by overwriting the timestamp. Also planned for Clients tab: same pattern applied to unpaid balances ("Jours depuis solde dû") so an invoice doesn't quietly age out unnoticed — offered to Emile, his answer on including this specific piece isn't locked in yet.

Emile also asked for a full quality pass once built: verify everything works, and make the UI good — "pretty and functional," not just correct.

- **Meta Ads lead automation:** leads from Meta ads currently have to be messaged manually every time — no webhook like the website form → Netlify → Apps Script pipeline. Meta has a Lead Ads API/webhook that could do this, but it requires a Facebook Developer app + page connection + Meta review, so it's a separate project, not a quick script tweak. Not started.

**Manual Meta Ads lead intake workflow (established 2026-08-20):** until that automation exists, Emile screenshots new leads from the Meta Ads tracker and emails them to the business inbox (duo.vert.gatineau@gmail.com), subject "Leads", one PNG per lead. Gmail's MCP connector here only exposes attachment *metadata* (filename/mimeType/id) — no tool to pull the actual image bytes — so screenshots have to be pasted directly into chat instead for me to read (see [[feedback/gmail-no-attachment-access]]). **Fields to log, exactly as given, never invented:** date, name, phone (usually present), email (if present, not critical), address (if present). Lead number doesn't matter to him. Provenance is always "MetaAd" for these. If a field isn't visible in the screenshot, leave it blank — don't guess or infer.

**Confirmed Pré-soumission column layout (2026-08-20, read directly from the live sheet):** A=# Lead, B=Date recue, C=Nom, D=Telephone, E=Courriel, F=Adresse, G=Langue (dropdown, e.g. FR), H=Provenance (dropdown, e.g. Formulaire/MetaAd), I=Moyen de contact (dropdown), J=Service, K=Details, L=Photo (URL), M=Follow-up 1 (checkbox), N=Follow-up 2 (checkbox), O=Date visite prévue, P=Soumission envoyée (checkbox), Q=Jours depuis réception (auto-calculated formula — never write to this column directly). Rows 1-3 are the header band (title/subtitle/actual headers); data starts row 4. **Adding new rows: don't type live into the sheet** (see [[feedback/build-locally-not-live-browser]] fifth incident) — build a headerless CSV in this exact column order and have Emile use File → Import → Upload → "Append to current sheet", which drops rows in cleanly at the bottom of whichever tab is active. **Confirmed working end-to-end (2026-08-20):** first post-import screenshot looked empty (Emile said "doesn't look like it worked") but that was just a stale/cached render — a follow-up check minutes later showed all 6 rows landed correctly with right # Lead, dates, names, and Provenance. Don't second-guess the method itself on one inconclusive screenshot; re-check before assuming failure.

## CRM consideration (raised 2026-08-17, not decided)

Emile is outgrowing the "remember everyone informally" stage as the company scales (running ads, doing door-to-door). Sheets (leads + expenses) work but feel disconnected from each other, and he's wondering whether he needs a real CRM — mentioned GoHighLevel by name as the model: unified inbox (email + SMS from one number), lead pipeline stages (not contacted / contacted no reply / active conversation / etc.), expense tracking, scheduling, automation, all in one place.

**Why this matters:** he explicitly asked whether I could build a fully-featured GoHighLevel-like CRM myself, for free. I pushed back — a real CRM (auth, database, SMS/email sending with deliverability, hosting) isn't a zero-cost build once a phone number or sending domain is involved, and recommended either paying for an existing tool (GoHighLevel/Pipedrive/HubSpot) or a lighter Sheets+automation hybrid using tools I already have access to (SMS/email sending MCP tools). No decision made yet — this is a live open question, not a plan.

**How to apply:** don't start building CRM infrastructure unprompted. If this comes back up, pick up from the "poor man's CRM vs. paid tool" comparison I offered.

**Root problem clarified (2026-08-17):** the actual pain isn't lack of a dashboard — it's that Emile and Beckett each text/call leads from their own personal phone numbers, so neither has visibility into what the other did with a given lead. This is a real operational gap for the two-person team. The one thing no free build (mine or otherwise) can fix is capturing texts/calls automatically without a shared business number — that requires both of them to send/receive through one shared line. Emile explicitly said to forget the "Sent" messaging tool for now ("you have to pay for that") — so no shared-number solution is active; the interim plan floated was a manual shared log (either of them jots a quick note after contacting a lead) plus a dashboard aggregating Sheets/Gmail/Calendar, which I can build for free. Revisit Sent (or another shared-number option) if/when they're ready to solve the automatic-capture piece for real.

## CRM decision revisited for 2027 (2026-08-27)

Emile raised the leads-tracking rebuild question again, this time with real stakes: for
2027 the sheet needs to work for **at least 4 concurrent people** (2 hired sales reps +
Emile + Beckett, possibly more), including reps entering leads live from a phone while
door-knocking. He described the current sheet as "breaks often" and asked me to weigh
in on GoHighLevel vs. rebuilding in Sheets vs. a custom-built private web app.

**My recommendation, given:** lean toward GoHighLevel (or re-testing it seriously via a
trial) over either rebuilding Sheets again or building custom. Reasoning: (1) the
recorded breakage above (silent `onEdit` failures, `moveRowPreToPost` bugs) stems from
bending Sheets + fragile Apps Script automation into CRM-shaped work it isn't built
for — adding 2 more users, especially ones entering data from a phone mid-door-knock,
makes that structural mismatch worse, not better; (2) this reconnects directly to the
root problem already identified below (personal-phone-number visibility gap) — that
gap gets more painful with 2 more people texting/calling from their own numbers, and
only a real shared-number platform (GHL, or the already-connected [[Sent]] messaging
tool) fixes it, no amount of better spreadsheet design can; (3) a custom-built private
web app is appealing for control but represents real ongoing engineering surface
(mobile UX, auth, reliability in the field with real money on the line during the
actual season) that existing CRM tools have already hardened — not recommended unless
something specific is needed that GHL/Sheets genuinely can't do. **Caveat given to
Emile:** my knowledge of GHL's current exact feature set/pricing may be stale since he
hasn't used it in months and the product changes — recommended he actually trial it
(sign up, test the pipeline + mobile lead entry for real) before committing, rather
than deciding from memory alone.

**Correction from Emile (same conversation, 2026-08-27): GHL is already decided,
independent of this discussion** — "using GoHighLevel isn't a decision I have to make,
it's already made, we will use GoHighLevel." The open question isn't adopt-or-not, it's
whether GHL alone covers everything they need or they'll need supplementary tools
alongside it. **Timeline:** they plan to start paying for GHL access around **May
2027** (a bit before the season starts, to leave setup time before hiring), and Emile
won't have any hands-on access to the platform before then.

**Full requirements list Emile gave 2026-08-27, to check GHL coverage against:**
1. **Leads** — richer pipeline stages than GHL's default categories (Emile's
   recollection is that default GHL stages are too generic, 2-3-word labels like "not
   contacted"/"no response" — wants more visual granularity than that), last-contacted
   date, and whether a quote was sent + the actual quote amount.
2. **Clients** — same contact info as leads, plus secure storage for the signed
   contract, plus paid/not-paid status.
3. **Materials cost tracking** — money spent on materials per job, needed to calculate
   the correct amount still owed by each client (see the existing solde-dû logic
   above) — explicitly framed as essential at scale (25+ clients planned).

**My assessment given 2026-08-27 (flagged as needing live verification, not taken as
certain):** leads/pipeline-stages/quote-amount/contract-storage/paid-status are core
CRM territory — a real CRM should let you define fully custom pipeline stages (not
locked to generic defaults) and custom fields per contact for things like quote amount,
plus GHL specifically is known to have proposal/contract and invoicing features that
should cover contract storage and paid/unpaid tracking. **Materials/expense tracking is
the one piece I'd recommend NOT forcing into GHL** — a CRM isn't accounting software,
and the existing [[duo-vert/sheets-tracking|Dépenses sheet]] already solves the
Emile/Beckett expense-split reconciliation well. Recommended approach: track "materials
cost for this job" as a single custom field per client record in GHL (keeps the
amount-owed math correct in the CRM), while the detailed receipt-level expense log
stays in the existing Dépenses sheet rather than migrating. **Not yet decided/verified
— still needs a real GHL session (trial or otherwise) to confirm the specific features
above actually work as expected**, since Emile hasn't used the platform in months and
it changes. Prep work that doesn't require GHL access and can happen now: finalizing
the exact pipeline stage names, exact custom fields needed, and what the contract
template should contain, so May is fast configuration rather than fresh design.

**Materials/equipment expense tracking — CONFIRMED staying outside GHL, 2026-08-27.**
Emile agreed volume is low enough ("we wouldn't have that much") that a Google Sheet
(or similar, format doesn't matter to him) is fine for this rather than building it
into GHL — matches the recommendation above. The existing [[duo-vert/company|Dépenses
sheet]] logic stays as the model for this, not a CRM feature.

**Parallel exploration, 2026-08-28:** separate from the GHL decision above, Emile is
also testing whether he can build his own custom CRM using Claude Code — see
[[duo-vert/custom-crm-prototype]]. Not a reversal of the GHL plan, a feasibility
experiment run alongside it.

See also: [[duo-vert/company]], [[feedback/build-locally-not-live-browser]],
[[duo-vert/season-2027-plan]] (2026-08-26: considering a GoHighLevel CRM that may
replace/extend this sheet for 2027), [[duo-vert/custom-crm-prototype]]
