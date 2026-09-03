---
name: duovert-custom-crm-prototype
description: "Emile testing whether he can build his own custom CRM (with custom integrations) using Claude Code, as a parallel exploration alongside the already-decided GoHighLevel plan"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-31
  originSessionId: 37c90df8-fa63-4f3a-b1db-ddc3345c606b
---

**Started 2026-08-28.** Emile said "I really want to try and build my crm, I'm sure we
can do some really good stuff." When asked to reconcile this against the already-locked
GoHighLevel decision (see [[duo-vert/sheets-tracking]], "CRM decision revisited for
2027" — GHL is decided, access starts ~May 2027), he clarified this is **not** a
reversal of that decision. He wants to "test if I can actually build my own, with
custom integrations, using Claude Code" — a feasibility exploration, run in parallel,
not a replacement commitment. Treat GHL as still the plan of record for the actual 2027
season unless he says otherwise; this prototype is a separate track.

**Scope decisions given 2026-08-28 (AskUserQuestion answers):**
- **Where it runs:** build locally first (`npm run dev` on his Mac). "Once it's ready to
  be posted, we'll host it so that all the team members can use" — so local-first now,
  real hosting/multi-user access is an explicit later step, not this build.
- **Integrations wanted:** "text messages, gmail, a calendar, trackers, and more that we
  will figure out" — an open-ended, evolving list, not one clean priority. SMS was the
  suggested/recommended starting integration (ties to the already-diagnosed root pain in
  [[duo-vert/sheets-tracking]]: Emile and Beckett text leads from personal numbers with
  no shared visibility — this is the one thing Sheets structurally can't fix and a real
  CRM/custom build can).
- **Data:** explicitly wants fake/seed test data for the prototype, not his real 18+
  live leads currently in the "Duo Vert - Suivi Leads" sheet — no risk to real client
  data while testing.

**Field/data requirements:** reuses the exact Leads + Clients field structure already
spec'd in [[duo-vert/sheets-tracking]] (the Pré-soumission/Post-soumission/Clients
columns, the profit-math rule that Prix du service is profit and Coût matériaux is a
pure pass-through, the paid-status/solde-dû calculated fields) rather than inventing a
new schema — same underlying business, same requirements list Emile already gave when
scoping GHL coverage. Materials/expense receipt-level tracking stays out of scope here
too, same as the GHL plan (stays in the separate Dépenses sheet).

**Architecture note worth remembering:** the MCP tools already connected in Claude Code
sessions (Gmail, Google Calendar, Sent for SMS/WhatsApp/RCS) are Claude's own tool
connections for chat — a standalone web app Emile and Beckett open in a browser does
NOT inherit these automatically. Any real integration needs its own API
credentials/OAuth app wired into the app's backend, separate from the Claude session's
MCP auth. Flagged as a setup dependency for whichever integration gets built first, not
something free.

**Repo location convention:** per [[personal/mac-file-organization]], new business repos
go under `~/Documents/Duo Vert/` alongside `duovert-site`/`duovert-print`/
`duovert-infra` — a new CRM repo should follow the same pattern (e.g.
`~/Documents/Duo Vert/duovert-crm`).

**Status as of 2026-08-28: Phase 1 shipped and verified.** Repo lives at
`~/Documents/Duo Vert/duovert-crm` (git-tracked, initial commit made). Emile pushed
back mid-planning on the first draft ("Contacts" mistakenly modeled as a team roster
instead of the master lead/client list) and on subagent use — both corrected before the
build; see [[feedback/avoid-subagents-for-hands-on-builds]] and
[[feedback/ask-detailed-specs-before-building]]. Everything below was built and
functionally verified directly (DB inspection, curl, real browser interaction via
Playwright/Claude Browser) — not just visually checked.

**Stack:** Next.js 16 (App Router, Turbopack) + TypeScript, Prisma 7 + SQLite (via
`@prisma/adapter-better-sqlite3`), Tailwind + shadcn/ui (this shadcn generation is
built on **Base UI**, not Radix — `asChild` doesn't exist, composition uses a `render`
prop instead, and `Select` needs an `items` map for the trigger to show a label instead
of the raw value; both bit the build once and are now fixed everywhere). dnd-kit for
the kanban, Recharts for charts (bar animation disabled — cosmetic-only Recharts quirk,
not a bug).

**Built and verified:**
- Pipeline (kanban, configurable stages, drag-and-drop) — verified via direct DB
  inspection after a real drag, correctly persisted stage + logged a STAGE_CHANGE
  activity.
- Contacts (master list, matching real GHL's structure) — table w/ search/filter,
  detail view w/ full field editing, save-and-verify round-tripped through SQLite.
- Quick Add Lead (door-to-door manual entry) — verified end-to-end, row created with
  correct source/stage/owner, test row cleaned up after.
- Team roster with per-person stats (active leads, won count, revenue).
- Reporting dashboard: funnel chart, revenue-by-service chart, closing rate, unpaid
  balances table with the correct profit/pass-through money math applied.
- **Gmail — real integration, not a stub.** Shared-account OAuth flow (`/settings` →
  `/api/auth/google/start` → Google consent → `/api/auth/google/callback`, refresh
  token stored in a `GoogleAccount` singleton row), send wired into the contact
  activity timeline. Cannot be fully tested end-to-end yet — needs Emile to create a
  Google Cloud OAuth app (steps are on the `/settings` page) — but the code path,
  UI states, and graceful "not connected yet" messaging are all verified.
- **SMS — real integration against Sent's actual v3 API**, looked up directly from
  docs.sent.dm rather than guessed (`POST https://api.sent.dm/v3/messages`, `x-api-key`
  header). Confirmed live during the build that the Sent account is signed up but
  unfunded ($0 balance) — matches the earlier note in [[duo-vert/sheets-tracking]] about
  deferring Sent. Code is ready; needs `SENT_API_KEY` once the account is funded, no
  code changes required at that point.
- Website lead webhook (`/api/webhooks/website-lead`) — accepts the same field names as
  the existing Netlify pipeline, requires a shared-secret header, has a 5-minute dedup
  guard. Fully curl-tested (auth rejection, flat payload, Netlify-nested payload,
  missing-field validation, dedup) — genuinely working code, just can't receive live
  traffic until this app is hosted with a public URL.

**Explicitly not built this pass:** Meta Ads lead automation (flagged as its own real
project, needs a Facebook Developer app + Meta review), real hosting/deploy, real
multi-user auth (currently an "acting as" selector, no password), object storage for
contracts (local filesystem only right now).

## Round 2 — visual redesign + Conversations/Training/Calendar (2026-08-29)

Emile's reaction to the Phase 1 prototype: "really good first start" but too bland/not
colorful enough (he described himself as "a really visual guy"), and three concrete
feature gaps. Asked me to go through the whole app and optimize, and to think about
what's missing given everything I know about the business — not just take his list
literally.

**Design direction confirmed:** real Duo Vert brand colors, not a generic palette.
Pulled the actual CSS custom properties straight from duovert.ca (`--g` #4caf50 primary
green, `--g2`/`--g-dark`/`--g-deep` shades, `--pale`/`--off` backgrounds, plus gold
#ffb300, red #e53e3e, and teal #115e59 accents already used on the site) rather than
inventing a palette — worth remembering this is a live, verified extraction, not a
guess, if the brand colors ever change on the site. Sidebar is now dark green, pipeline
stage colors form a deliberate progression (neutral grey → teal → gold → orange → red,
branching to green for won).

**Conversations tab** — built to match GHL's actual unified-inbox pattern he described:
search leads by name, filter by stage, see the message thread, compose and send via
Email or SMS from one screen. Lives at `/conversations`, alongside (not replacing) the
per-contact activity log on the Contacts page, per his explicit choice.

**Formation (Training) section** — `/training`, an empty-structure content library
(title/category/link/description) ready for whenever real SOPs get written. Directly
answers the training-materials gap already tracked in
[[duo-vert/season-2027-plan]] section 6 ("not planned in detail yet... hands-on vs.
formal still undecided") and the Oct–Nov 2026 prep window in section 12 — this CRM
section is where that content should land once written, not a replacement for actually
writing it.

**Calendrier section** — `/calendar`, real scheduling (not just a read-only view):
month grid, create/edit/delete events, optional link to a contact. Chose "real
scheduling" explicitly over the lighter read-only option when asked.

**A real bug worth remembering for future component work on this stack:** shadcn's
`Select` (Base UI-based, see the earlier `asChild`→`render` note) needs an `items` prop
on the `Select` root or the trigger displays the raw stored value instead of the
matching label — hit this again while building the Calendar/Training dialogs, same root
cause as the Team dropdown bug from round 1, now fixed everywhere consistently.

**Status:** production build verified clean (`tsc`, `eslint`, `next build` all pass),
every new feature tested end-to-end via direct database checks, seed data confirmed
clean (24 contacts, no test rows left behind). Committed to git.

## Round 3 — Calendar rebuild, team chat, map, branding fixes (built 2026-08-29)

Full plan approved via plan mode 2026-08-29, saved at
`~/.claude/plans/ok-first-of-all-drifting-wozniak.md`. **Status: built and
verified same day** — schema migrated, `tsc`/`eslint`/`next build` all pass
clean, every feature checked via direct browser interaction (Claude Browser
tooling) not just visual spot-check: overlap rendering confirmed by creating
two real overlapping events, crew filter toggle confirmed hiding/showing
events, chat message send + lead-tag-to-Activity-timeline sync confirmed
end-to-end, map pin click-through to contact detail confirmed. Committed to
git (commit "Round 3: fix font, color pipeline columns, rebuild calendar, add
team chat and map"). One real bug hit and fixed during the build: Leaflet
measures its container size once at mount and can catch a flex layout mid-resize,
rendering only a small tile patch — fixed with a ResizeObserver-based
`invalidateSize()` call in `map-view.tsx`, worth remembering for any future
Leaflet-in-flexbox work. Geocoding backfill on the 24 seed contacts only
succeeded for 6 (synthetic fake addresses don't all resolve through
Nominatim) — expected and fine for a prototype, flagged in the plan
beforehand as a real production-address concern would be much lower.

Confirmed requirements (not assumptions, locked in via AskUserQuestion):

- **Font:** CRM's font-sans var was actually a bug (`--font-sans: var(--font-sans);`
  circular reference in `globals.css`, silently falling back to browser default
  instead of the intended Geist). Fixing to match the website's real heading font,
  which is **Inter** (weight 900/font-black on H1, not a separate "display" font
  despite the CSS variable being named `--font-display` on duovert-site).
- **Pipeline columns:** `PipelineStage.colorHex` already existed in schema and was
  already a deliberate color progression, just never applied to column
  backgrounds/borders (only a small dot). Fix is a rendering-only change,
  `stage-column.tsx`.
- **Calendar:** one unified calendar for the whole team (explicitly NOT
  per-person/per-role separate calendars) — everyone can self-book, overlapping
  events across crews are expected and must render side-by-side, filterable by
  crew (Sales/Cleaning/Owners) or individual via checkboxes. Every event needs:
  1+ attendees, a task type from a fixed dropdown in this exact order —
  **Porte-à-porte, Soumission, Nettoyage, Sablage, Scellant, Inspection finale**
  (Inspection finale only really applies to Emile/Beckett) — 0+ linked leads, and
  free-text notes. Day/week/month view toggle required (current build was
  month-only). Decision: build the day/week grid from scratch rather than adopt
  react-big-calendar/FullCalendar, since their layout engines assume one
  color/resource per event and fight the multi-attendee/multi-lead model needed
  here — flagged as the single largest engineering chunk of this round.
- **Team roles:** `Role` enum only had `OWNER`/`REP` — no sales/cleaning
  distinction existed. Extending to `OWNER | SALES | CLEANING` to support the
  crew-filter checkboxes on the calendar (existing `REP` seed users map to
  `SALES`; no cleaning-crew users existed yet, added as new seed data).
- **Team chat:** brand new feature, nothing existed to extend (no
  Conversation/Message model at all — separate from the existing lead-facing
  "Conversations" SMS/email tab, which is Activity-model-backed, not a real chat
  model). New nav tab named "Discussion": one GENERAL channel for everyone + 1:1
  DMs. Confirmed: tagging a lead inside a chat message ALSO logs an Activity note
  on that lead's own timeline (not chat-only) — reuses the existing Activity/NOTE
  type, zero changes needed to `ActivityTimeline`. Real-time approach is polling
  (~4-6s) + refetch-on-focus, not websockets — explicitly acceptable for this
  team's scale.
- **Map:** Emile flagged this as one of the most important tabs to get right —
  "the exact real map," not invented/stylized. Provider decision:
  **OpenStreetMap via Leaflet/react-leaflet**, not Google Maps. Reasoning worth
  remembering: Emile initially believed his own website already had a free
  Google Maps embed and could work the same way in the CRM — verified this was
  wrong, duovert-site has no map embed at all, only a text link out to Google
  Maps. The interactive pins-and-hover-and-click map he wants needs the Google
  **JavaScript** Maps API specifically (not the free Embed API), which requires a
  Google Cloud billing card on file — he chose to avoid that account-setup step
  entirely and go with OSM instead, after seeing a live OSM screenshot of
  Gatineau to confirm the visual style was acceptable. Pins color-coded by
  `PipelineStage.colorHex` (same colors as pipeline columns, for consistency),
  hover shows name+stage, click navigates to the contact detail page. Needs
  geocoding (contacts only store free-text addresses, no lat/lng) via Nominatim
  (OSM's free geocoder, rate-limited to ~1req/sec per its usage policy).
  Street/route-coverage drawing for door-to-door tracking is explicitly deferred
  ("something we could explore" later), not part of this build.
- **Explicitly deferred to later** (Emile's own words: "integrations are for
  later"): phone calling via the business number, Meta Ads/website auto-lead
  webhook integration (existing `/api/webhooks/website-lead` route may be
  reusable with an adapter), all-team SMS visibility, owner-only expense
  tracking, owner-only employee/performance tracking. Also flagged as
  suggestions only (from GoHighLevel/SPOTIO/Knockbase research, not requested by
  Emile): per-address knock-outcome tagging on the map, a combined "today's
  route" view merging calendar+map for field crews, role-based pipeline
  visibility if the team grows.

### Round 3 fixes (same day, 2026-08-29)

Two real usability bugs Emile caught right after using it, both fixed and
verified:
- Calendar Début/Fin fields committed on every native datetime-local tick
  with no confirmation, reading as "did nothing happened" when scrubbing the
  time. Fixed with a small popup (`datetime-field.tsx`) that only commits on
  an explicit "Confirmer" click.
- Root cause of "only 2-3 leads show on the map": hand-typed addresses in
  Quick Add Lead / contact edit silently failed to geocode with no feedback.
  Fixed with a live Nominatim address-autocomplete (`address-autocomplete.tsx`)
  on both forms — picking a real suggestion guarantees a geocodable address
  and skips a redundant geocode call (coords come straight from the
  suggestion). Old contacts with addresses typed before this fix still need
  their address re-entered through the new autocomplete to actually place a
  pin.

### Round 3 fixes, take 2 (same day, 2026-08-29)

Emile's first fix attempt didn't fully work; real root causes found and fixed:
- **Address autocomplete returned nothing for partial words / missing "rue"
  prefix.** Cause: Nominatim's `/search` only matches whole tokens, so it's
  unsuitable for live type-ahead. Switched to **Photon**
  (photon.komoot.io, free, OSM-based, no API key, purpose-built for
  autocomplete/prefix matching) for the live-suggestion lookup specifically —
  `geocodeAddress` (final exact geocode, backfill script) stays on Nominatim
  since that's a one-shot lookup of a complete address, not a fit issue.
  Worth remembering: **Nominatim ≠ good autocomplete source**, Photon is the
  right free OSM tool for that job specifically.
- **Time picker dropdown closed the whole popup on selecting an hour.**
  Cause: the hour/minute `Select` component (Base UI) renders its option list
  in a React portal outside the popup's DOM subtree, so the popup's
  click-outside handler saw the click as "outside" and closed everything.
  Fixed by excluding `[data-slot="select-content"]` from the outside-click
  check. Also replaced the native datetime-local input inside the popup with
  a plain date input + hour/minute dropdowns (no nested native time widget at
  all) per Emile's clarification that he wanted the confirm button "inside
  the little box," not a second native picker layered on top of it.

### Research pass — what to improve/build next (2026-08-29, no changes made)

Emile asked for research only ("sans rien changer") on what could be improved
or added. Findings, not yet acted on:

**Real risks before real team use:**
- No real login yet (cookie-based "acting as" dropdown) — recommended fix:
  **Better Auth** (current standard Next.js auth lib, magic-link support,
  first-class Prisma integration).
- `dev.db` is a local file on Emile's Mac — won't survive hosting. Recommended:
  **Turso** (SQLite-compatible cloud DB, free tier then $4.99/mo, plugs into
  Vercel directly, no Prisma schema change needed since it's still
  SQLite-wire-compatible).
- `Document` model (contracts) saves to local filesystem — evaporates on any
  serverless host (Vercel included). Needs real object storage (Cloudflare R2
  free tier is the natural fit) before contract storage is real.

**Feature ideas surfaced, prioritized by Emile's own pick of top 3:**
1. **Weather flags on the calendar** (free Open-Meteo API, no key) — nettoyage/
   scellant can't happen in rain, this is specific to paver work not generic
   CRM advice. Picked as first priority: cheapest, most specific, no
   dependencies.
2. **Real login via Better Auth** — picked second: not exciting but becomes a
   real problem the moment anyone besides Emile/Beckett opens the app.
3. **E-signature on quotes/contracts via DocuSeal** (open-source, free,
   self-hostable — deliberately not a paid DocuSign-style sub, fits the
   internal-tool-first build). Picked third.

Also surfaced but not prioritized: automated post-job review-request text
(ties to the existing Google-review-reply pattern in
[[duo-vert/revenue-growth-plan]], triggers off `projectEndDate` once Sent is
funded), a lightweight no-login client portal per quote (approve/ask a
question without a phone call — the feature Jobber/QuoteIQ lean on hardest),
and door-to-door knock-outcome tracking with 2-3-tap logging (reinforces the
map roadmap item from Round 3, sourced from Knockbase/SPOTIO's actual UX
pattern, not just "add a map feature").

## Round 4 — real auth, quote/contract generator, map drawing, chat groups (started 2026-08-29)

Emile asked for five things in one go: real login, a quote/contract generator
built off his existing templates, door-to-door line-drawing on the map
(tagged by who drew it), visibility into which shared number/email a message
sent from, and self-serve group-chat creation (not just the fixed
GENERAL/DM channels from Round 3). Also cleared up a real confusion: he heard
"Turso $4.99/mo" as "$499/mo" — corrected in chat. Netlify (free) hosts the
app; Turso (free tier, or $4.99/mo) is the separate cloud database piece,
needed because the current `dev.db` SQLite file won't survive any real host.

**Confirmed specs (AskUserQuestion, 2026-08-29):**
- **Auth flow — not magic link, not open self-registration:** owner
  generates an invite link, invitee opens it and sets an email + password,
  then **stays logged in indefinitely** (no repeated sign-in prompts). Built
  as: `Invite` model (token, email, name, role, expiry) generated from
  /settings (owner-only section, "Équipe — invitations"), `/invite/[token]`
  accept page creates/updates the `User` row with a scrypt password hash
  (Node's built-in `crypto.scryptSync`, no bcrypt dependency added),
  `Session` model backing a 180-day cookie. Real `/login` page replaces the
  old no-auth "acting as" dropdown entirely.
- **Map line history: keep everything**, not latest-only. A `RouteLine` row
  is never overwritten — re-covering a street later in the season is a new
  row, so coverage can be filtered by date/salesperson later. Matches
  Round 3's own deferred note about door-to-door route tracking.
- **Messaging "from" identity:** confirmed one shared Gmail + one shared SMS
  number for the whole team (not per-person), so the from-indicator is
  mainly a "this went out from the real business identity" confirmation, not
  a per-person distinction — scope accordingly, don't over-build per-user
  identity tracking that doesn't match reality yet.
- **Group chat creation: anyone**, not owners-only — any logged-in team
  member can create a group and manage its members, added `GROUP` to the
  existing `ChannelType` enum (alongside GENERAL/DM) plus a `name` field on
  `Channel`.

**Auth: built and verified end-to-end.** `prisma migrate reset --force` ran
with Emile's explicit consent (Prisma gates this when it detects an AI agent
driving the CLI — the exact command and consequences were confirmed in chat
first, not just inferred). Real browser login tested via Claude Browser:
`emile@duovert.ca` / `beckett@duovert.ca`, seeded password `duovert2027` —
meant to be replaced by a real self-invite once other real people are
onboarded, not a permanent credential. Invite flow, session cookie (180-day),
and the owner-only "Équipe — invitations" panel on /settings all work.

**Quote/contract generator: built and verified end-to-end.** Emile sent the
real template docs directly (Google Docs links for the soumission and
contrat templates — the same ones documented in
[[duo-vert/soumission-template]]) rather than needing them drafted from
scratch, so this wasn't the legal-content-authoring risk originally flagged
— it's transcription into a PDF template, verified byte-for-byte against the
source docs via `pdftotext`. Built with `@react-pdf/renderer` (pure-JS, no
headless-browser dependency — matters for Netlify/serverless later) at
`src/lib/pdf/quote-pdf.tsx` and `contract-pdf.tsx`, generated via server
actions in `src/app/actions/documents.tsx`, stored under `storage/documents/`
(gitignored) with a `Document` row (now has `language` and `dataJson` —
JSON snapshot of the input values, for future re-editing) served through an
auth-checked `/api/documents/[id]` route. UI lives on the contact detail
page's "Documents" card — "Soumission"/"Contrat" buttons open a dialog that
prefills name/address/phone from the contact, computes the 15%
avant+arrière discount automatically, and lets you pick FR or
"English (brouillon)". Per Emile's explicit call: **English is drafted now,
to be checked later** — not held back — but the generated EN contract PDF
carries a visible footer disclaimer ("Draft English translation — not yet
legally reviewed... especially the warranty article") so it can't
accidentally go to a real client unreviewed. FR quote/contract have no such
disclaimer since they're a direct verbatim match to Emile's approved
templates.

**Real gotcha hit this round — do not `rm -rf .next` while another `next
dev` is running in the same folder.** Multiple sessions/dev-server instances
against this one repo directory is now a recurring pattern (flagged again in
[[personal/dev-environment]]-adjacent territory) — deleting the `.next`
build cache out from under a live dev server corrupts its in-memory manifest
state and it does NOT self-heal; it needs the process killed and restarted
cleanly. Hit this twice in one session (once mid-build-verification, once
after a later build). Before running `next build` or clearing `.next`,
check `ps aux | grep "next dev"` first for a live process in the same repo.

**Also hit and worked around:** `@react-pdf/renderer`'s `renderToBuffer`
fails under `tsx`'s strict Node-ESM resolution
(`ERR_PACKAGE_PATH_NOT_EXPORTED` on `@react-pdf/hyphenate`) even though it
works fine inside Next's own bundler/runtime — don't treat a `tsx` script
failure on this package as proof the real app is broken; verify through the
actual running Next server instead.

**Follow-up spec, 2026-08-29 (plan-mode session, not yet built):** the
quote/contract dialogs need editable name/address/phone(/email) fields —
prefilled from the Contact record but overridable in the dialog itself, not
just silently pulled from whatever's on file. Reason: leads from the website
or Meta Ads sometimes arrive with incomplete info (only a first name, or
only an email with no phone) — Emile wants to be able to fill in the gap
right there when generating the document rather than having to go edit the
Contact record separately first. Also reported a real bug hit while testing:
a Next.js dev error overlay ("Base UI: A component is changing the default
value state of an uncontrolled FieldControl") appeared when generating a
quote — root cause not yet confirmed (investigation in progress), but worth
noting Emile hit an actual failure on his first real attempt, not just a
cosmetic console warning assumption.

## Round 5 — editable doc fields, map drawing, group chat, from-indicator, click-to-call, attachments (built + verified 2026-08-29)

Root cause of the "Base UI uncontrolled FieldControl" warning confirmed (via
direct DB/log inspection, not guessing): cosmetic only — the quote PDF had
already generated successfully; the warning fired because the quote-number
input's `defaultValue` prop changed after `revalidatePath` refreshed the
page while the dialog was still mid-close-animation. Fixed by switching
those fields to controlled state reset on the dialog's own `onOpenChange`
callback (not a `useEffect`, which a newer eslint rule
(`react-hooks/set-state-in-effect`) correctly flags as the wrong pattern for
"reset state when this opens" — same class of prop-timing issue as the
original bug, worth remembering for any future dialog-reset code here).

All seven pieces of this round shipped and were verified against the real
running dev server (not just typecheck/build): editable
name/phone/address fields on the quote/contract dialogs, which now also
write back to the Contact record on generate (confirmed choice: corrections
made here are permanent, not one-off); door-to-door map line drawing
(click-to-add-point polyline, "who drew this" picker defaulting to the
current user, colored/filterable by team member, no new npm dependency);
group chat creation (`createGroupChannel`, a "+" dialog on /discussion using
the same `MultiSelectList` the lead-tag picker already used); a shared
"from" indicator on /conversations (added a `BusinessSettings` singleton
table for the SMS number, `819-328-2129`, editable from /settings; Gmail
address was already stored, just not surfaced before); click-to-call
(`tel:` links next to the phone field and the "Appel" button — real
in-browser calling would need a separate paid telephony service like Twilio
Voice, explicitly deferred per Emile: "keep it simple for now"); real email
attachments (added `nodemailer` for `MailComposer`, since hand-rolling
MIME attachment encoding is easy to get subtly wrong); and SMS document
links via a new public, unauthenticated `/d/[token]` page + matching
`/api/public-documents/[token]` route (a `Document.shareToken` — generated
lazily on first use for documents predating this feature, or eagerly for
new ones), since Sent's API has no attachment/media field at all — confirmed
by fetching its actual docs, not assumed. `proxy.ts`'s auth-bypass matcher
was extended to exactly these two new public paths, nothing broader.

**Real gotcha hit again this round:** the dev server needs a full restart
(not just `.next` staying intact) after `prisma generate` regenerates the
client — a long-running `next dev` process keeps the OLD generated client
in its module cache, so a schema field added mid-session (here:
`Document.shareToken`) throws `Unknown argument` in Prisma calls until the
node process itself is killed and restarted. `rm -rf .next` alone does not
fix this; the process restart is the actual fix.

**PDF visual polish pass (2026-08-29, after a real generated quote was
tested):** Emile caught a real layout bug by generating an actual quote and
looking at it — the "DUO VERT" heading visually overlapped the contact line
right under it, and the quote wasted so much vertical space that the
signature section spilled onto an almost-empty second page. Fixed both
(tighter/corrected spacing) and used the opportunity to redesign the whole
PDF look while keeping every word of text identical to the approved
templates: Duo Vert green (#4caf50/#2e7d32) as an accent on section headers
and a divider under the logo, client-info and cost sections boxed with a
light green background, the final price highlighted in bold green, real
drawn signature lines instead of underscore blanks. A normal quote now fits
on one page. **Worth remembering for any future PDF/document template work
here:** Emile's bar for these client-facing documents is real visual
polish ("a client sees this and thinks... I wanna hire them") — verify
generated PDFs visually (render to an image, actually look at it), not just
via text extraction, since text-only checks miss layout/overlap bugs
entirely.

**Hosting still not acted on:** Netlify (free) + Turso (free/$4.99/mo) +
Cloudflare R2 (for Document PDFs) remains the researched plan; the app still
only runs locally.

See also: [[duo-vert/sheets-tracking]], [[duo-vert/season-2027-plan]],
[[personal/mac-file-organization]], [[project-current-todo-list]],
[[personal/dev-environment]], [[feedback/avoid-subagents-for-hands-on-builds]],
[[feedback/ask-detailed-specs-before-building]],
[[feedback/legal-content-needs-permission]]

**UX pass (2026-08-29, same day as the PDF polish pass):** Emile flagged four
usability gaps from actually using the CRM day to day, all now built and
browser-verified:
- **Back-navigation:** "Voir la fiche" from Conversations lost your place —
  no way back except re-searching the lead. Fixed generically: the selected
  contact now lives in the URL (`/conversations?contact=id`), and every
  list→detail link (Conversations, Contacts table, Pipeline, Map pins,
  Discussion, Reporting) carries a `?from=` param that a new `BackLink`
  component on the contact page reads to render "← Retour" back to the
  exact same spot.
- **Map:** added an address search bar (reuses the existing
  `AddressAutocomplete`/Photon autocomplete already built for quote/contact
  address fields) that recenters the map on selection. Route lines are now
  clickable → popup with who/when + Modifier/Supprimer. **Permission rule
  Emile gave explicitly:** anyone can delete their own line; only OWNER-role
  users (Emile + Beckett) can delete someone else's — enforced server-side,
  not just hidden client-side.
- **AI drafting assistant — chose Gemini over Claude, deliberately:** Emile
  wants it "free," doesn't know/care about providers ("I just want it to be
  free, I've had like a free AI assistant in websites before"). Claude's API
  has no free tier; Gemini does (aistudio.google.com/apikey, no credit
  card) — Emile already has Google AI Studio familiarity from the photo
  workflow work (see [[duo-vert/photo-workflow]]). Built on `@google/genai`,
  model `gemini-2.5-flash`. House voice encoded in the system prompt: casual-
  professional, human, no em dashes (see
  [[feedback/no-em-dashes]]), never "au plaisir de vous lire" (see
  [[feedback/sms-draft-formatting]]), pulls real conversation history
  (last 15 activities) for context. **Not yet usable — GEMINI_API_KEY is
  still empty in `.env.local`.** Emile needs to grab a free key and paste it
  in a future session before the button (currently shows disabled with an
  explanatory tooltip, verified working) lights up.
- **Quotes & Contracts:** new dedicated "/documents" tab (nav label
  "Soumissions & Contrats") listing every generated document across all
  clients with delete (removes DB row + the PDF file on disk). The existing
  quote/contract generator dialogs (already built, Emile called them
  "clean and professional") are now also embedded directly in the
  Conversations thread header, not just buried on the contact page.
  Generating one auto-selects it as the message attachment and hands the AI
  assistant document context so the draft naturally references "je viens de
  t'envoyer la soumission."

Provider-choice pattern worth remembering generally: when Emile says he
wants something "free" without naming a provider, don't default to
Anthropic/Claude just because that's this session's own ecosystem — check
what actually has a real free tier and what he already has an account for.

**AI assistant now live (2026-08-29, later same day):** Emile pasted a free
Gemini API key (from aistudio.google.com/apikey, named "Duo Vert CRM" in his
Google AI Studio project) — added to `.env.local` as `GEMINI_API_KEY`, dev
server restarted to pick it up. Hit and fixed a real model-availability bug:
`gemini-2.5-flash` is no longer available to new API keys/users (Gemini API
returned a 404 telling us to use it) — the model constant in `src/lib/
gemini.ts` is now `gemini-3.6-flash`. Verified with a real generated SMS
draft, on-brand and human. **Worth remembering generally:** Gemini model
names shift under new keys faster than training data reflects — if a future
Gemini integration 404s on a model name, check the error message itself
first (it names the replacement) rather than guessing a version.

**Mobile support is now a standing requirement, not a nice-to-have (2026-08-29):**
Emile confirmed both he and employees (sales reps, cleaners) will use the CRM on their
phones sometimes/often ("it's guaranteed"). Any future UI work on this CRM should be
checked at mobile width (use the Browser pane's `resize_window` to `mobile`, 375x812)
before considering it done, not just at desktop width. The visual-polish/mobile pass
this round covered: Map overlay spacing (search bar was see-through, controls
collided), Conversations AI-assistant-panel overflow, and a full mobile nav (sidebar
collapses to a hamburger + drawer below `md`, Conversations goes list-then-thread with
a back arrow). Contact detail, tables, and the Pipeline kanban board already degraded
acceptably on mobile without changes.

**Exact filename template for quotes/contracts (confirmed 2026-08-29):**
`Soumission Duo Vert - {fullName}.pdf` (FR quote), `Quote Duo Vert - {fullName}.pdf`
(EN quote), `Contrat Duo Vert - {fullName}.pdf` (FR contract), `Contract Duo Vert -
{fullName}.pdf` (EN contract) - no quote number, no language suffix in parens. If this
naming pattern comes up again for other document types, this is the established style
to match.

## Round 6 — scoping ads tracking + Instagram/Facebook messaging (planned 2026-08-29, not yet built)

Emile asked for brainstorm input on what to add next; wants "automations and as
much integrations as I can that are obviously useful" plus ad/website tracking.
Surfaced ideas: ads-vs-revenue ROI dashboard, expense tracking, stage-based
automations (review-request SMS, cold-lead alerts, weather flags), QuickBooks
sync, e-signature, client portal.

**Decision: build real Meta integration (ads tracking + Instagram/Facebook
messaging) directly into the custom CRM, not via Vibiz.** While scoping, found
that a marketing platform called **Vibiz** (MCP tools available in Claude Code
sessions) is already connected to Emile's `duo.vert.gatineau@gmail.com` account
under a "Duo Vert's portfolio" workspace (confirmed via `vibiz_whoami`) and has
built-in Meta/Google/TikTok ads analytics plus a unified Instagram/Facebook
inbox — but the workspace shows 0 accessible vibiz, i.e. never actually set up.
Flagged this to Emile as the fast alternative before committing to a multi-week
custom build (Instagram/Facebook DM messaging needs a Meta Developer App +
Meta's app review process to work for real, which realistically takes weeks).
**His call, explicit:** unsure if Vibiz is free/worth it, and "I have lots of
time so maybe having the real thing is good" — proceed with the custom build
inside the CRM, ads tracking AND IG/FB messaging both, accepting the Meta
review timeline. Ads tracking (read-only reporting APIs, no review bottleneck)
builds into the CRM's Reporting tab. IG/FB messaging integrates into the
existing /conversations unified inbox alongside Email/SMS, once Meta's
Developer App + review clears.

Note for future sessions: if Emile ever asks about Vibiz again or wants a
faster path to social messaging, remind him this workspace already exists
and just needs setup — don't re-discover it from scratch.

**Round 6 build: shipped and verified same day (2026-08-29).** Scope confirmed
via AskUserQuestion: ads tracking (Meta + Google) AND the IG/FB messaging code
both, in one pass; GA4/Search Console explicitly deferred to a later session.

Schema: `AdAccount`/`AdCampaign`/`AdSpendDaily`/`AdPlatform` enum for ads;
`MetaAccount` singleton (mirrors `GoogleAccount`) plus `Contact.metaPsid`/
`metaIgsid` and `INSTAGRAM_DM`/`FACEBOOK_DM` activity types for messaging.
Applied via `prisma db push --accept-data-loss` (not `migrate dev`, which
wanted a full reset over pre-existing drift unrelated to this change) — this
needed Emile's explicit typed consent twice: once for Prisma's own AI-agent
safety gate, then again because Claude Code's auto-mode classifier separately
blocked the command even with consent passed through, requiring Emile to
switch out of auto mode for that one step. Worth remembering: Prisma's
consent gate and the harness's own classifier are two independent blocks: satisfying one doesn't satisfy the other.

New lib files: `meta.ts` (shared OAuth, one Meta app covers both ads_read and
messaging scopes), `meta-ads.ts`, `google-ads.ts` (new `google-ads-api`
npm dependency; Google Ads reports `cost_micros`, divided by 10,000 for
cents — NOT the same conversion as Meta's native minor-unit spend), `meta-messaging.ts`. New webhook `/api/webhooks/meta` (GET handshake +
HMAC-SHA256 `X-Hub-Signature-256` verification on the POST body, raw text
read before JSON.parse per Meta's requirement) — auto-creates a placeholder
Contact on first inbound DM (name filled in manually later, same as the
existing door-to-door quick-add pattern). Extracted the previously-inline
`SOURCES` list in `contact-detail-form.tsx` into a shared
`src/lib/lead-sources.ts` (`LEAD_SOURCES` + `PLATFORM_LEAD_SOURCE` mapping)
so the ads-vs-revenue join in Reporting can't silently fragment on
inconsistent free-text values.

Verified for real, not just typechecked: webhook signature accept/reject +
GET handshake accept/reject via curl with a real HMAC-signed payload (created
a real Contact+Activity row, confirmed via direct DB query, then cleaned up);
Reporting's ads charts + CPL/CPC math verified against seeded mock spend
joined against the real 8 MetaAd-sourced seed contacts (1 won) — $6.25
CPL/$50 CPC came out correct; Settings/Conversations pages checked via
browser text extraction (logged in as emile@duovert.ca); mobile width
(375×812) checked via `scrollWidth`, no horizontal overflow. `tsc`/`eslint`/
`next build` all clean.

**Not yet usable — needs external setup Emile hasn't done:** a Meta Developer
App (`META_APP_ID`/`META_APP_SECRET`/`META_REDIRECT_URI`/
`META_WEBHOOK_VERIFY_TOKEN`), a Google Cloud Ads API enablement +
developer token (`GOOGLE_ADS_DEVELOPER_TOKEN`/`GOOGLE_ADS_REDIRECT_URI`), and
— before real IG/FB messages can flow — Meta's App Review approval on
`pages_messaging`/`instagram_manage_messages` (the multi-week external
process flagged throughout planning). All connection cards in Settings show
the exact setup steps inline.

**Google Ads setup deliberately deferred (2026-08-29):** Emile confirmed there's
no live Google Ads account right now (the campaign from
[[duo-vert/google-ads-campaign]] is still drafted, not launched — pending
Beckett's card). Told him to skip the Google Ads developer-token/Cloud-API
setup steps entirely until that campaign actually goes live — no point
provisioning API access with nothing to sync yet. Only did the Meta setup
steps (Business verification + Developer App) this round. The Google Ads
code is already built and waiting; revisit the setup steps once the campaign
launches.

**Meta OAuth connection — debugged and resolved end-to-end (2026-08-29).** Created a Meta Developer App ("Duo Vert CRM", App ID `2076667483235970`) under the Duo Vert Business Portfolio with Marketing API, Messenger, Instagram messaging, and "Facebook Login for Business" (found under classic Products, not the use-case gallery). `META_APP_ID`/`META_APP_SECRET`/`META_REDIRECT_URI` set in `.env.local`; localhost redirects work automatically in Development mode, no manual allow-list needed.

Two early bugs fixed on the first connect attempt: (1) every `render={<Link .../>}` connect button was missing `nativeButton={false}` (Base UI's `Button` defaults `nativeButton` true and misbehaves with a non-`<button>` render target) — worth remembering for any future connect-style button using this pattern; (2) `instagram_manage_messages` was rejected as an invalid scope on the first OAuth attempt — Instagram DM sending needs an Instagram account linked as a tester first, so the scope was pulled out temporarily (ads tracking + Facebook Messenger proceeded without it).

**Real architecture lesson, the expensive one:** "Facebook Login for Business" and classic "Facebook Login" are NOT interchangeable despite the similar name — the Business variant needs a **Login Configuration** (App Dashboard → Facebook Login for Business → Configurations) yielding a **Configuration ID**, and the OAuth URL needs `config_id=<that ID>` instead of a plain `scope=` param. Final Configuration: `ads_read`, `pages_show_list`, `pages_messaging`, `pages_manage_metadata`, `instagram_basic`, `leads_retrieval`, `pages_read_engagement` (deliberately excluded `ads_management`/`pages_manage_ads`/`business_management` — no built feature uses them yet, and Meta App Review scrutinizes write-permissions with no matching feature; cheap to add later once a real feature needs one), access token type **User access token**, Configuration ID `1374487200930344`.

Even with `config_id` wired in, the connection kept failing with "no Page found" — a long debugging chain (~10 back-and-forth rounds, `auth_type=rerequest` tried and reverted since it made Facebook *skip* the picker instead of forcing it; the real app-revoke location turned out to be `facebook.com/settings?tab=business_tools`, not `tab=applications`, which doesn't list Business-Portfolio-connected apps at all) eventually isolated the real cause via temp debug logging: **`/me/accounts` does not list Pages owned by a Business Manager** (as opposed to a personally-created Page) — a known Graph API edge case. The Duo Vert Page is owned by the business, not by Emile personally, so the standard "list my accounts" call always returned empty even with every permission correctly granted and the Page correctly selected in the consent picker (which, separately, only ever shows once per grant — revoking is required to see it again, not any OAuth URL parameter).

**Fix:** fetch the Page directly by its known ID (`GET /1228204477033457`) instead of listing via `/me/accounts`. Rewrote `exchangeCodeForMetaAccount` accordingly, added `META_PAGE_ID=1228204477033457` as a required env var. Connection confirmed working end-to-end via direct SQLite query (`MetaAccount` row has the real pageId/pageName/linked Instagram id) and a real ad-account auto-discovery. **Worth remembering broadly for any future Meta Graph API integration on a Business-Manager-owned Page: don't rely on `/me/accounts` to discover the Page, fetch it directly by its known Page ID instead** — this single finding would have skipped roughly ten rounds of debugging if known upfront.

**First real sync, same day:** "Synchroniser les pubs" on `/reporting` pulled real data confirming Duo Vert has a **live, currently-running Meta Ads campaign** ("New Leads Campaign," $346.51 spent over 8 days, 23,153 impressions, 426 clicks) — not just the drafted-but-unlaunched Google Ads one from [[duo-vert/google-ads-campaign]]. End of session: Meta integration (ads tracking + messaging groundwork) fully live and verified, only Meta App Review remains before Instagram/Facebook DMs work for real.

## Round 7 — ad-performance detail + GA4/Search Console (built same day, 2026-08-29)

Emile pushed back after seeing the thin initial ads output ("was that a lot
of work for almost nothing?") and asked for real ad-performance analysis
plus website traffic tracking (GA4 + Search Console), which had been
deferred as "Part C" earlier this session.

**Ad-performance detail (built + verified against real synced data):**
`SpendOverTimeChart` (line chart, daily spend) added to `ads-charts.tsx`.
Reporting page now also computes and shows: overall CTR/CPC stat tiles,
Meta's own reported conversions vs. the CRM's actual received leads (a real
comparison — surfaced as "9 de moins que Meta" live, since Meta reports 17
conversions but only 8 leads are tagged MetaAd in the CRM, a genuine
tracking-gap signal), and a full per-campaign table (spend/impressions/
clicks/CTR/CPC/CPM/status) — which incidentally revealed the real campaign
"New Leads Campaign" is currently **PAUSED**, worth flagging to Emile if not
already known. All computed from data already synced (impressions/clicks/
conversions per day were already being pulled from Meta's API, just never
surfaced in the UI before this round).

**GA4 + Search Console (built, NOT yet connected/verified live):** both
`google.analyticsdata` and `google.searchconsole` namespaces already exist
in the installed `googleapis` package — no new npm dependency needed,
unlike Google Ads. New `SiteAnalyticsAccount` singleton model (own OAuth
grant, not reusing `GoogleAccount`, same reasoning as Google Ads — different
scope grant, possibly a different Google account than the shared Gmail
inbox). New `src/lib/site-analytics.ts`, OAuth routes at
`/api/auth/site-analytics/{start,callback}`, `TrafficOverTimeChart` +
`TopQueriesTable` components, a Reporting section (guarded so a Google API
failure just hides the section rather than breaking the page), and a
Settings connection card with full setup instructions (enable Google
Analytics Data API + Search Console API on the same Google Cloud project as
Gmail, add the callback redirect URI, find the GA4 property ID and exact
Search Console site URL, set `SITE_ANALYTICS_REDIRECT_URI`/
`GA4_PROPERTY_ID`/`SEARCH_CONSOLE_SITE_URL`). `tsc`/`eslint`/`next build`
all clean, Settings/Reporting pages checked live — card renders correctly,
Reporting correctly shows nothing extra since not connected yet. **Not
connected or live-tested** — Emile hasn't done the Google Cloud setup steps
yet, so the actual GA4/Search Console data pull is unverified pending that
(reasonable to expect this hits its own round of Google-specific gotchas
the way Meta did, given the pattern this session).

**Real finding while starting the Google Cloud setup (2026-08-29): there is
no Google Cloud OAuth Client at all yet.** Emile's Google Cloud Console
(project "My First Project", id `subtle-bus-507102-f6`) shows zero OAuth 2.0
Client IDs under Credentials. This means Gmail's OAuth was never actually
set up either, consistent with Settings showing "Gmail: Non connecté"
throughout every session so far - the earlier assumption that GA4/Search
Console/Google Ads could just "reuse the same Google Cloud project as
Gmail" was correct on the project level, but there was no existing OAuth
client to add a redirect URI to. Redirected the plan: create ONE new OAuth
Client covering all three redirect URIs at once (Gmail, Google Ads, Site
Analytics) so this whole setup only has to happen once instead of three
separate times. Emile is mid-flow: OAuth consent screen configuration
first, then creating the actual OAuth Client. Not yet complete as of this
entry - once he has a real Client ID/Secret, `GOOGLE_CLIENT_ID`/
`GOOGLE_CLIENT_SECRET` need adding to `.env.local` for the first time ever
in this project (all three Google integrations depend on it, not just site
analytics).

**Google Cloud OAuth Client created, real credentials live (2026-08-29).**
First attempt's client secret never displayed (Google Cloud Console quirk -
sometimes the secret creation silently fails or the reveal UI doesn't show
up); deleted that client and recreated a second one, which worked normally
(secret shown once in a popup right after creation, as expected). Real
Client ID/Secret now set in `.env.local` as `GOOGLE_CLIENT_ID`/
`GOOGLE_CLIENT_SECRET`, with all three redirect URIs
(`GOOGLE_REDIRECT_URI`/`GOOGLE_ADS_REDIRECT_URI`/
`SITE_ANALYTICS_REDIRECT_URI`) registered on the one client. **This also
means Gmail can finally actually be connected** for the first time in this
project's history - it was never configured before this (see the earlier
finding that zero OAuth clients existed). Still needed before Site Analytics
fully works: the GA4 Property ID and Search Console site URL (steps 3-4 from
the original checklist, not yet done - conversation got sidetracked into the
OAuth Client creation detour). Google Ads still separately needs its own
developer token whenever that campaign launches, per the earlier deferral.

**GA4 Property ID and Search Console site URL confirmed, all env vars now
set (2026-08-29):** `GA4_PROPERTY_ID=548164587`,
`SEARCH_CONSOLE_SITE_URL=sc-domain:duovert.ca` (read directly off Emile's
Search Console address bar, `resource_id=sc-domain:duovert.ca`, rather than
making him hunt for it in the UI). Full `.env.local` config for Site
Analytics is complete, server restarted. Not yet clicked "Connecter" /
verified live as of this entry - that's the next actual step. Gmail is also
now connectable for the first time (same shared Google Cloud Client), still
unconnected as of this entry too.

**OAuth connection succeeded, but data pull failed — real root cause found
(2026-08-29).** Emile got through Google's "unverified app" gate by adding
himself as a Test user on the OAuth consent screen's Audience tab (in the
new Google Auth Platform UI, not the old "OAuth consent screen" layout — the
Homepage/Privacy Policy/Terms fields he initially found belong to the
publish/verification flow, not needed for testing mode). `SiteAnalyticsAccount`
row confirmed created for real (`duo.vert.gatineau@gmail.com`, correct GA4
property, correct Search Console URL) — but the Reporting page's new section
never appeared. Added temporary debug logging (same pattern as the Meta
saga) and found the real cause: **Google Analytics Data API was never
actually enabled** on the Google Cloud project (577525041225) — likely lost
in the earlier detour where Step 1 of the checklist (enabling both APIs) got
skipped once the "no OAuth Client exists" problem took over the
conversation. Direct fix link: console.developers.google.com/apis/api/
analyticsdata.googleapis.com/overview?project=577525041225 → Enable.
Also improved the code while debugging: switched
`Promise.all` to `Promise.allSettled` in `reporting/page.tsx` for the
GA4/Search Console fetch, since the original code would silently hide
whether Search Console's API had the same problem whenever GA4 failed first
(Promise.all short-circuits on first rejection) — now each API's
success/failure is independent and separately logged. Not yet confirmed
working end-to-end as of this entry — Emile about to enable the API and
retry.

**Search Console CONFIRMED working with real data (2026-08-29).** The
`Promise.allSettled` fix immediately paid off: after enabling the Analytics
Data API, Search Console's own call succeeded independently (it had been
enabled correctly all along) and real query data now renders on Reporting —
actual top queries for duovert.ca: "duo vert" (18 clicks), "nettoyage pave
uni gatineau" (5 clicks), "duo vert gatineau", "duovert", "duo vert gatineau
reviews", "paysagiste aylmer", plus several zero-click ones. GA4 sessions
chart still empty — Google's own error said API enablement can take a few
minutes to propagate; Emile is waiting and will retry. Once GA4 catches up,
this integration is fully done.

**GA4 CONFIRMED working too — Round 7 fully complete (2026-08-29).** After
the propagation delay, GA4 sessions chart renders real data (verified via
screenshot, not just trusting the screen): a real daily-fluctuation line
chart, dates Aug 1-28, matching duovert.ca's actual traffic. Both GA4 and
Search Console are live and correct. Debug `console.log` calls left in
(reworded from "TEMP DEBUG" to permanent `console.error` graceful-
degradation logging, since the `Promise.allSettled` pattern itself is worth
keeping permanently — if one Google API ever breaks again, this surfaces it
in server logs instead of silently and confusingly hiding half the section).
`tsc`/`eslint`/`next build` all clean on the full `src` tree, server
restarted clean.

**Full end-of-session status:** Meta (ads tracking + Facebook Messenger
groundwork) live and verified. GA4 + Search Console live and verified.
Google Ads still deferred until that campaign launches. Google Cloud OAuth
Client now exists for the first time ever in this project, unlocking Gmail
too (not yet connected, but no longer blocked). Instagram DM sending still
needs the tester/account-linking fix for `instagram_manage_messages`, and
real IG/FB messaging still needs Meta App Review. This was a long,
debugging-heavy session (three separate real external-API integrations, each
with its own non-obvious platform quirk) but ended with every piece that
could be verified locally actually verified working end-to-end, not just
code-complete.

## Round 8 — deep-dive Reporting tabs for Meta/GA4/Search Console (built + verified, 2026-08-29)

Emile felt the initial reporting build was "a lot of work for almost
nothing" given how thin the numbers looked — asked for everything possible
from the three connected sources, with plain-language explanations next to
every chart. Went through plan mode. Confirmed via AskUserQuestion: not
separate pages — a tabbed interface all still on `/reporting` (Résumé / Meta
/ Google Analytics / Search Console), and explicitly skipped Facebook/
Instagram Lead Ads auto-import for this round (needs a real webhook-
subscription setup step, and zero payoff while ads are paused — permission
already granted, quick add later).

**Built, zero new external setup needed** (reused every existing
connection): `/reporting` rewritten as a `Tabs` layout (shadcn/Base-UI,
already in the codebase). Résumé tab keeps the original KPI/funnel/revenue/
balances content. New Meta tab adds: age/gender demographics breakdown,
placement breakdown (Feed/Stories/Reels/Marketplace), and a "Ta Page
Facebook" card (followers + post engagements, via the Page Insights API —
a different Graph API surface than Ads Insights, new `src/lib/
meta-page-insights.ts`). New Google Analytics tab adds: traffic sources,
device split, top pages, new-vs-returning. New Search Console tab adds:
per-page performance, device split, country breakdown, and a clicks/
impressions trend chart. Every chart/table got a `MetricNote` caption
(new shared component) explaining what the number means and roughly what to
aim for — general well-known guidance, not invented precise benchmark
stats, per house style on not fabricating cited numbers.

**Two real bugs found and fixed via actual browser+build verification, not
just clean typecheck:**
1. **`page_impressions_unique` Page Insights metric is deprecated** — Meta
   removed it without much warning; the combined `metric=a,b` call was
   failing entirely because of it. Fixed by splitting the Page Insights call
   into independent `Promise.allSettled` pieces (`fan_count` vs.
   `page_post_engagements` separately) so one future metric deprecation only
   drops that one stat, not the whole Page card. `PageEngagementStats.
   postEngagements30d` is now nullable and the UI hides that tile if absent.
2. **GA4 new-vs-returning chart showed 4 bars instead of 2** — GA4 can
   return more rows than just "new"/"returning" (e.g. an extra
   "(not set)" bucket); the original code mapped 1 API row -> 1 chart bar
   without aggregating same-label rows. Fixed by summing into exactly two
   buckets before returning from `getGa4NewVsReturning`.

**Also caught and fixed live on mobile (375×812, standing requirement):**
the new `TabsList` didn't fit four tab labels on a phone width — "Search
Console" was cut off with no way to reach it (page itself didn't overflow,
just the tab bar). Fixed with `overflow-x-auto flex-nowrap` on the list and
`shrink-0` on each trigger, verified by actually scrolling the tab bar via
JS and confirming `scrollLeft` moved and the label became fully visible.

**Verified for real per tab, not just build-clean:** screenshotted all four
tabs with real data — Meta (real follower count "2", real per-age/gender
click tooltips, real placement spend ranking), Google Analytics (real
traffic-source breakdown: Direct/Organic Search/Organic Social/Paid, real
top pages with view counts), Search Console (real per-page CTR/position
table showing which of the 10+ city pages actually get search traffic vs.
which don't, real device/country tables). `tsc`/`eslint`/`next build` all
clean on the full `src` tree throughout.

## Ideas for next session (2026-08-29, end of day — recap below; digest + instant auto-reply shipped in Round 9, digest advisory layer in Round 10, see below)

Emile signed off for the night with two feature ideas for a future session,
explicitly "probably tomorrow":

**1. AI daily digest tab.** A dedicated CRM tab with an AI-generated
end-of-day recap: new leads (count + names), stage changes, a plain-language
summary of each conversation thread that day (not raw messages — e.g. "we
talked to Michael and booked a quote for [date]"), ad/site performance that
day, and a summary of the team's physical field activity pulled from the
Calendar (which crew did what, for how long, at which address). Motivation:
once Beckett and other employees are using the CRM too, this becomes the
shared way everyone stays in sync day to day without re-reading everything
themselves. Feedback given: genuinely buildable with the already-connected
Gemini API (same one used for the AI draft assistant) reading that day's
`Activity` + `Event` records — no new integration needed. Suggested it be
browsable by date (a real archive), not just "today." The field-activity
part is only as good as the team actually filling in Calendar events
accurately (attendees + task type), which is already the intended workflow
from Round 3.

**2. Automations.** Two ideas, both new: (a) instant auto-reply (SMS + email)
the moment a new lead comes in, so nobody waits before getting a response;
(b) a follow-up nudge after N days (Emile said ~23 as a hypothetical) of no
reply, via whichever channel the conversation was already happening on.
Feedback given: build (a) first — clear trigger, universally useful, low
risk. For (b), recommended starting as an AI-drafted suggestion a person
reviews and sends rather than a fully automatic send, since a wrong-feeling
automated nudge to a real client reads as spammy in a way a new-lead welcome
message doesn't — can graduate to fully automatic later once the drafts have
been seen in practice. Emile also asked generally for "any other suggestions
to make this the best CRM" — open standing invitation, nothing else proposed
yet.

This was a save-for-next-session note, end of a long day (Rounds 6-8 all
happened in one sitting). Idea 1 (daily digest, including the trend/advisory
refinement below) and idea 2(a) (instant auto-reply) shipped in Round 9/10
below. Idea 2(b) (follow-up nudge after N days of no reply) is still open —
not built in either round.

**Refinement to the daily digest idea (same conversation, right before
sign-off):** the AI shouldn't just recap the day — it should look at trends
over the last 1-2 weeks and surface proactive suggestions, e.g. "engagement
is dropping, follow up more often" or "not enough new leads from ads lately,
consider adjusting targeting or increasing budget." Emile's framing: these
are suggestions he might not act on, but wants surfaced daily so they're not
missed. This turns the digest from a pure recap into a lightweight advisory
layer — a real differentiator, worth building into the same feature rather
than as a separate one.

**Other CRM ideas surfaced (not requested by Emile, offered proactively,
none built or scoped yet):** speed-to-lead tracking (response time to a new
lead, a strong real-world predictor of conversion), stale-lead alerts (a
lead untouched for N days flagged somewhere visible — ties into the
automations idea above), referral-source tracking (capture "referred by
[client]" specifically rather than generic word-of-mouth, to enable a future
referral-thank-you/incentive flow), post-job review request automation
(already on the list, ties to the existing Google-review-reply pattern in
[[duo-vert/revenue-growth-plan]]).

Emile explicitly said he's new to this and wants ongoing research/
suggestions on how to use AI well in the CRM, not just build-on-request —
worth proactively bringing ideas in future sessions, not only reacting to
what he asks for.

## Round 9 — instant auto-reply + AI daily digest (shipped and verified 2026-08-30)

Confirmed via AskUserQuestion: build both ideas flagged at the end of Round 8.

**Gmail is actually connected** — checked directly via `sqlite3 dev.db`, the
`GoogleAccount` row exists with a real refresh token for
`duo.vert.gatineau@gmail.com`. This resolves the "not yet connected" status
noted at the end of Round 7/8 — somewhere between then and now Emile (or a
session) completed the Gmail OAuth connect. Emile himself wasn't sure it was
connected; worth re-verifying state directly rather than trusting memory or
his own uncertainty going forward.

**Instant auto-reply — confirmed specs:**
- **Fully automatic**, no human review step (matches the original
  recommendation: low risk for a first-touch welcome message, speed-to-lead
  matters).
- **Email is definite** (Gmail, confirmed connected above). **SMS via Sent is
  undecided** — Emile is unsure he'll keep using Sent specifically, floated
  wanting to shop other phone-number/SMS options for the CRM. Explained to
  him and confirming here: the trigger + Gemini-drafting logic is
  provider-agnostic: only the final "send an SMS" call is Sent-specific and
  already isolated in its own function, so building it now against Sent
  risks zero wasted work if he switches providers later — only that one
  function would need swapping. Decision: build the full mechanism now,
  attempt both channels, log "not sent, channel not connected/funded"
  gracefully per channel rather than failing — matches the existing
  `Promise.allSettled`-style graceful-degradation pattern already used for
  Meta Page Insights and Google Analytics/Search Console in Round 7-8.

**AI daily digest — confirmed scope:** recap only this round (new leads,
stage changes, plain-language conversation summaries, ad/site performance,
field activity from Calendar), browsable by date. The trend/advisory layer
(1-2 week analysis, proactive suggestions) explicitly deferred to a later
round once the recap itself is verified working — not dropped, just
sequenced after.

Implementation plan being drafted this session, not yet built.

**Round 9 build: shipped and verified same day (2026-08-30).** Both pieces
built directly (no subagents, per standing preference) and verified against
the real running dev server + direct SQLite queries, not just typecheck.

**Auto-reply (`src/lib/auto-reply.ts`):** hooked into `quickAddLead()` and
the website-lead webhook (not the Meta path, per the plan's reasoning - no
channel exists yet on a placeholder-named DM contact). Reuses
`draftMessage()` from `ai-draft.ts` (same house-voice system prompt) with a
new welcome-message instruction, attempts email (Gmail) and SMS (Sent)
independently, each wrapped in its own try/catch so one failing channel
never blocks the other or the lead-creation call. Sends/logs directly via
`gmail.ts`/`sent.ts` rather than importing `sendEmailActivity`/
`sendSmsActivity` from `contacts.ts` - `auto-reply.ts` is imported BY
`contacts.ts`, so the reverse import would have created a circular module
reference. New `BusinessSettings.autoReplyEnabled` field (via `prisma db
push`, no consent gate needed this time since it's a purely additive
nullable-safe boolean column, not a destructive change) with an OWNER-gated
toggle card in Settings, same upsert/action pattern as the existing SMS
number card.

**Verified via a real curl against the website-lead webhook** (to
`duo.vert.gatineau@gmail.com`, so delivery could be checked in the CRM's own
inbox): the whole pipeline ran correctly end-to-end except the very last
step - Gemini drafted a real, on-brand French welcome message, but the
actual Gmail send failed with a 403 because **the Gmail API itself isn't
enabled** on the Google Cloud project (577525041225), a separate one-time
step from having a connected OAuth refresh token (same category of gap as
the Round 7 "Analytics Data API never enabled" finding). The graceful
per-channel degradation worked exactly as designed: the STAGE_CHANGE
activity still got created, the webhook still returned `ok:true`, and the
error was caught and logged instead of crashing anything. **Confirmed fully working end-to-end same day** - Emile enabled the Gmail
API, a follow-up curl test produced a real EMAIL Activity with a genuine
`gmailMessageId`, and the send was independently confirmed via the Gmail
connector (`search_threads` found the exact thread, sender/recipient
`duo.vert.gatineau@gmail.com`, snippet matching the approved wording
verbatim). Auto-reply email is now genuinely live, not just code-complete.
SMS still blocked on Sent being funded, as always. Test contacts cleaned up
after each verification pass.

**AI daily digest (`/digest`, `src/lib/digest.ts`):** recap-only as scoped -
new leads, stage changes, one batched Gemini call summarizing all of a
day's conversation threads at once (not one call per contact, to keep it to
a single API call regardless of how many leads had activity that day),
Calendar field activity, and ad spend/impressions/clicks summed from the
already-synced `AdSpendDaily` table (no new live API calls, keeps the page
fast). Date navigation via `?date=YYYY-MM-DD` query param, prev/next/today
links, `?from=/digest` on every contact link following the existing
BackLink convention. Added to nav (`Newspaper` icon, "Résumé du jour") in
the single shared `NavLinks` component used by both desktop sidebar and
mobile sheet - no duplicate edit needed. Verified against a real historical
date (2026-08-19) with actual seeded data: the Gemini-generated summary for
a real CALL activity read correctly ("Michel Lavoie — Premier contact
téléphonique effectué et le client s'est montré intéressé"), real ad spend
numbers rendered ($37.31, 2561 impressions, 47 clicks). Checked at mobile
width (375×812, standing requirement) via `scrollWidth` - no horizontal
overflow, cards stack correctly.

`tsc`/`eslint`/`next build` all clean on the full tree.

**Trend/advisory layer for the digest remains deferred**, as scoped - not
built this round.

**Spec change, same day (2026-08-30): the auto-reply's first-touch message
should be a fixed template, not AI-drafted per lead.** Emile: "I always want
the exact same message except for the name to be sent initially" - one fixed
SMS text and one fixed (different, can be longer) email text, both with
just the lead's name substituted in, no Gemini call for this specific
message. Drafted both with him, refining his own proposed SMS wording
("Salut {name}, c'est Emile de Duo Vert - Restauration de pavé uni. Merci
pour votre demande de soumission, on a de la place dans les prochains jours
pour venir évaluer les travaux. Avez-vous une préférence de journée?") plus
a parallel email version (subject "Merci pour votre demande - Duo Vert",
slightly longer body, signed "Emile / Duo Vert", no "au plaisir de vous
lire" per [[feedback/sms-draft-formatting]]). **Confirmed and built same day.** `auto-reply.ts` no longer calls
`draftMessage()`/Gemini for this message at all - `firstName()` extracts the
first token of `Contact.fullName`, then two plain template functions
(`welcomeEmailBody`, `welcomeSmsText`) substitute it into the exact approved
wording. Settings card copy updated to say "le même message... juste le
prénom qui change" instead of "rédigé par l'IA", and the
`isGeminiConfigured()` gate was removed entirely from `sendAutoReply` (no
longer needed since this path doesn't touch Gemini). Verified via a second
real curl against the website-lead webhook (contact "Marie Tremblay" →
decoded MIME body read back exactly "Bonjour Marie, ..." with the agreed
wording verbatim) - only failure was the same pre-existing Gmail API
enablement gap, unrelated to this change. `tsc`/`eslint` clean, test contact
cleaned up. Worth remembering generally: Emile may want future first-touch/
templated messages to work the same way (fixed text + name substitution),
not everything needs to be AI-drafted - he was explicit that the AI-drafted
approach was NOT what he wanted for this specific message.

## Round 10 — digest trend/advisory layer, shipped and verified 2026-08-31

Confirmed via AskUserQuestion, picking up the layer deferred at the end of
Round 8/9 (1-2 week trend analysis, proactive suggestions like "engagement
dropping" or "not enough leads from ads"):
- **Computation: hybrid.** Code computes the real trend numbers (leads this
  week vs last, CPL/ad trend, stale leads, pipeline movement) so nothing is
  hallucinated; Gemini only turns each triggered trend into a natural-
  language sentence, following the same pattern as `summarizeThreads` in
  `src/lib/digest.ts`.
- **Signals: all four** proposed — lead volume trend, ad performance trend
  (CPL/CPC/spend), conversation responsiveness/stale leads (folds the
  separately-floated stale-lead-alert idea into this feature instead of
  building it standalone), and pipeline movement (stuck-in-stage, closed
  this week vs last).
- **Placement: inside the existing `/digest` page**, shown daily as a new
  card, not a separate weekly page — reasoning was it's already a standing
  daily habit, so suggestions get seen without a new page to remember to
  check.

**Scope expanded mid-planning (2026-08-31): website analytics pulled in too.**
Emile clarified he wants "everything at one place" — not just CRM leads/ads/
pipeline but also GA4 and Search Console, with real comparisons (day/week/
month) and AI commentary that's grounded in real numbers, explicitly "not
just like AI slop." Confirmed via a second AskUserQuestion round: comparison
windows use a smart default per metric rather than showing all three windows
for everything (landed on week-over-week for every signal this round, month-
over-month ranking movement deferred); website analytics joins the CRM
signals in one unified card, grouped by topic, not a separate card; and each
triggered signal gets an explicit grounded recommendation line (built the
same hybrid way, never generic advice unconnected to the real numbers).

**Round 10 build: shipped and verified same day (2026-08-31).** Built
directly (no subagents for the actual implementation, per standing
preference — Explore/Plan agents were used only for the pre-build research/
design phase while still in plan mode). Six signals live in a new
`src/lib/digest-trends.ts`: lead volume, ad CPL/spend-without-leads
(reusing new `src/lib/ads-metrics.ts`, extracted from the previously-inline
math in `reporting/page.tsx` — refactor confirmed behavior-preserving,
identical Meta tab numbers before/after), stale leads (absorbs the
separately-floated stale-lead-alert idea), pipeline movement, and two new
website signals (GA4 sessions, Search Console clicks) via two new
explicit-date-range functions added to `site-analytics.ts`
(`getGa4SessionsInRange`/`getSearchConsoleClicksInRange`) so trend windows
correctly follow the digest's viewed date instead of always meaning "today."
New "Vue d'ensemble" card on `/digest`, grouped by topic (Publicité,
Pipeline, Site web, Leads), each triggered item showing a fact plus a
grounded recommendation as a muted second line, plus the stale-lead list.

Verified against real synced data, not just typecheck: real numbers
confirmed correct across multiple dates (e.g. 2026-08-20 showed Meta CPL
$126.81 vs $92.89 the week before, GA4 sessions 79 vs 10, 8 stale leads
oldest at 20 days — all cross-checked against `/reporting`'s existing
numbers for the same window), date-navigation confirmed to shift every
signal's window correctly, mobile width (375×812) confirmed no overflow.
`tsc`/`eslint`/`next build` all clean.

**Real bug hit and it proved the fallback design works, not a code bug:**
while testing, Gemini's API returned a genuine transient 503 ("model
currently experiencing high demand") — the phrasing call's try/catch caught
it exactly as designed and the digest fell back to the plain-template French
sentences instead of breaking or blanking the page, then recovered to real
Gemini phrasing on the very next request once the API responded normally
again. Worth remembering as a real-world confirmation of the
`summarizeThreads`-style graceful-degradation pattern this feature followed,
not a hypothetical concern.

**Deferred, not built this round:** search ranking/position month-over-month
movement (needs a bigger `site-analytics.ts` extension for per-query
matching across two periods) — flagged as a good next addition once the
week-over-week signals are proven out in daily use.

**Sparked a bigger idea (2026-08-31):** seeing this CRM come together is what
prompted Emile to float [[personal/agency-idea]] — selling website/CRM builds
to other businesses using Claude Code, with this CRM as the reusable
template/proof of concept. Not a change to this prototype's own scope, just
noting the connection.

## Round 11 — pipeline sort-to-top fix + AI help chatbot, scoped 2026-08-31 (plan mode, not yet built)

Two asks in one session. **(1) Pipeline stage-sort bug:** when a lead's stage
changes on the kanban board, it lands at the bottom of the destination column
instead of the top — makes it hard to spot the most recently-moved lead. Root
cause found: no per-card order field exists, columns sort by `createdAt`
(never changes on a move), and the optimistic client-side update appends
rather than prepends. Planned fix: add `Contact.stageChangedAt`, set it in
`moveContactStage`, sort pipeline columns by it, prepend instead of append
client-side.

**(2) AI help chatbot — new feature, not requested before.** A floating
button bottom-right of every screen, click to open a small popover chat.
Purpose: instant help on how to use the CRM's features/navigation — Emile
built the whole thing himself and knows it, but even he can forget things,
and it's flagged as especially useful for **Beckett**, who didn't build the
CRM and has more to learn navigating it. Explicit scope: no persisted/
remembered conversation history needed, just quick straight-to-the-point
answers to an immediate question — not a general assistant, a nothing-fancy
help lookup. Planned to reuse the existing Gemini integration
(`src/lib/gemini.ts`, `gemini-3.6-flash`, same pattern as `ai-draft.ts`'s
`draftMessage`) rather than a new AI dependency, with a system prompt
describing every CRM section/workflow so it can answer "where do I find X" /
"how do I do Y" questions grounded in the app's actual structure.

Emile also asked for a broader research pass on other CRM improvements,
prompting a deeper look at what's already been surfaced-but-not-built across
this file (weather flags on Calendar, e-signature via DocuSeal, client
portal, referral-source tracking, speed-to-lead tracking, post-job
review-request automation, Meta/Facebook Lead Ads auto-import, search-ranking
month-over-month trend) to present back as an options list — not a specific
new idea generation pass, a synthesis of the backlog already accumulated in
this same file across Rounds 1-10.

**Round 11 build: shipped and verified same day (2026-08-31).** Both pieces
built directly (no subagents for the implementation, per standing
preference), verified against the real running dev server, not just
typecheck/build.

**Sort-to-top:** added `Contact.stageChangedAt` (via `prisma db push`, clean),
set in `moveContactStage` on every real stage change, pipeline query now
orders by it instead of `createdAt`, and the optimistic client-side move in
`kanban-board.tsx` now prepends instead of appends. Verified via a real drag
in the browser (Sophie Bergeron, Nouveau lead → Contacté) landing at the top
instantly, then confirmed still at the top after a full page reload (proves
the server-side sort persisted, not just the optimistic UI).

**AI help chatbot:** new `src/app/actions/help-chat.ts` (`answerHelpQuestion`,
mirrors `ai-draft.ts`'s pattern exactly — same `getGeminiClient()`/
`GEMINI_MODEL` call, graceful `isGeminiConfigured()` guard that returns a
message instead of throwing) and `src/components/shared/help-chat-widget.tsx`
(hand-rolled floating-button + popover, following the existing
`ai-draft-button.tsx` pattern since no `popover.tsx` primitive exists in this
codebase), mounted in `src/app/(app)/layout.tsx` so it's on every
authenticated page but never `/login` (structurally guaranteed by the route
group, not just a runtime check). Fully stateless as scoped: each question is
an independent Gemini call with a static system prompt describing every real
CRM section (Pipeline/Contacts/Digest/Conversations/Discussion/Calendar/Map/
Documents/Team/Reporting/Training/Settings) — no DB model, no persisted
history, the visible message bubbles are just local React state that vanish
on reload. Verified with a real question in the browser ("comment j'envoie
une soumission à un client?") — answered correctly, pointed to the real
Soumissions & Contrats /documents section. Checked at mobile width, popover
stays within `calc(100vw-2rem)`, no overflow. `tsc`/`eslint` clean on the
full tree.

The options-list synthesis (weather flags, e-signature, client portal,
referral tracking, etc.) was presented back to Emile for feedback, not acted
on — no decision made yet on which (if any) to build next.

## Round 12 — performance investigation and fix (2026-08-31)

Emile noticed the CRM felt laggy and worried it'd be slow once actually
hosted. Measured real page-load times via the browser (not guessing):
almost every page was already fast (60-120ms warm), but **/digest was taking
28-34 seconds** and /reporting ~2.5s, both 100% "application-code" time per
Next's own server logs (not framework/compile overhead).

**Root cause found, not assumed:** two real bugs in `src/lib/
site-analytics.ts` / `src/lib/digest-trends.ts`.
1. `websiteSignals()` in digest-trends.ts fetched GA4 sessions then Search
   Console clicks **sequentially** (two separate try/await blocks) instead of
   together - unlike `/reporting`'s already-correct `Promise.allSettled`
   pattern for its 10 GA4/SC calls. Fixed to match: one combined
   `Promise.allSettled`.
2. **Bigger find:** `authorizedClient()` created a brand-new `OAuth2Client`
   instance on every single GA4/Search Console call, so each call
   re-negotiated a fresh access token via the refresh_token instead of
   reusing one - googleapis's OAuth2Client caches its access token
   internally, but only if you reuse the same client instance. /reporting
   fires ~10 of these calls per load, /digest fires 4 - each was paying its
   own token-refresh round-trip. Fixed with a module-level cached client
   instance (keyed by refresh token), shared across calls in the process.

**Real, measured impact:** /digest first load after the fix: 11.9s (still
real external API+Gemini work), but **every load after that within the same
process: 450-750ms** (was 28-34s every time before). /reporting: 2.5s → ~0.8-1.2s.
Verified /digest still renders correct data after the refactor (real
signals: lead-volume drop, stale-lead list, the stage changes from earlier
testing all showed up correctly).

**Also addressed the actual underlying worry** ("scared it'll be slow on a
real site"): built and ran a real production build (`next build` + `next
start`) side by side with the dev server. Confirmed concretely, not just
asserted: dev mode (Turbopack) adds real per-request overhead even on
already-compiled routes (Next's own dev-mode request handling, HMR
bookkeeping) - the same static /login page took 20-97ms in dev vs 1.5-2.2ms
in production, a real ~10-50x difference measured via curl on both. Told
Emile plainly: most of the CRM was never actually slow, dev mode has its own
baseline tax that goes away entirely in production, and the two real bugs
found (both now fixed) were the only genuine app-level slowness - not
something that would have surfaced differently or been masked once actually
hosted, since Google/Gemini API latency exists in prod too.

Hosting still not acted on (Netlify + Turso + Cloudflare R2 remains the
researched plan from Round 3's research pass) - this round was diagnosis +
fix of real measured slowness, not a hosting-readiness pass.

**Real production blocker found the same round: Gemini free tier is a hard
20 requests/day cap, shared across every AI feature.** Emile hit a 429
`RESOURCE_EXHAUSTED` error on `/digest` right after this perf work (partly
from the perf-testing reloads themselves, partly his own testing). Confirmed
via dev server logs this was NOT a crash - the existing
`[digest-trends] Gemini phrasing failed` try/catch in `phraseSignals()`
caught it exactly as designed and fell back to plain-template French text,
page kept working. But the underlying limit is real and matters for going
live: `generativelanguage.googleapis.com/generate_content_free_tier_requests`
is capped at **20/day total per model** (`gemini-3.6-flash`), and this quota
is shared across ALL Gemini call sites in the app - the help chatbot, AI
draft assistant, daily digest (up to 2 calls per single page load), and
auto-reply drafting all draw from the same 20/day pool, not 20 each. Resets
daily. Real day-to-day use (team opening the digest + asking the help bot
questions + drafting messages) will exhaust this fast, likely within an
hour some days - this doesn't get better once hosted, the same wall just
gets hit by more people. Offered to walk him through enabling billing on the
Google AI Studio project whenever he's ready; he said hold off for now.

**Real free fix found and applied same day, no billing needed.** Emile asked
if there's any way to get good-quality AI for free at all. Checked current
2026 rate-limit info via web search rather than relying on stale training
data: Gemini's free tier is generally much more generous than 20/day
(multiple 2026 sources cite ~1,500 requests/day for "Flash" and
"Flash-Lite" tier models) - the 20/day cap Emile hit is specific to
`gemini-3.6-flash`, the newer flagship-generation model this app happened
to be pinned to (from an earlier round's `gemini-2.5-flash` deprecation
fix), which gets a much stricter free allowance than Google's lighter
"-lite" model variants. Verified directly against Emile's real API key
(listed available models, test-called several candidates) rather than
guessing: `gemini-2.5-flash-lite` is dead (deprecated for new users, same
pattern that killed `gemini-2.5-flash` before), but `gemini-3.5-flash-lite`
works and is the exact model Google's own API error message names as the
current replacement. Switched `GEMINI_MODEL` in `src/lib/gemini.ts` from
`gemini-3.6-flash` to `gemini-3.5-flash-lite` (one-line change, comment
explains why). Verified end-to-end: reloaded `/digest` four times in a row
with zero 429s (previously erroring every single time), and the AI-phrased
trend text came back genuinely on-brand and grounded (e.g. "Relancez
rapidement les prospects de la semaine dernière..." instead of the plain
fallback sentence) - not just "no error," actually working AI output. Quota
size for `gemini-3.5-flash-lite` on this specific key isn't confirmed to the
exact number (would require exhausting it to know for sure), but the model
class matches what current sources describe as the ~1,500/day tier, a huge
improvement over 20/day. No other file hardcoded the old model name, so this
was the single fix point for every Gemini call site (help chat, AI draft,
digest, digest-trends, auto-reply) at once. Worth remembering generally: a
"free tier too restrictive" complaint on any Gemini integration is worth
checking model TIER (flagship vs lite) before assuming billing is the only
fix - and always verify actual model availability/quota against the real
key rather than trusting a model name from memory, since Google deprecates
specific model versions for new users fairly often (this is now the second
time in this project alone).

**Follow-up question, same day: Emile asked about self-hosting his own AI
("I've heard people... use GitHub to have their own AI with no limits").**
Researched rather than answered from memory. The specific thing he'd heard
about is real but dead: **GitHub Models** (free API access to GPT-4.1,
Llama 4, Mistral, DeepSeek etc. via a GitHub account, no card needed) was
**retired July 30, 2026**. Separately checked real self-hosting economics
(running an open model like Llama yourself via Ollama): break-even vs. a
hosted API only happens around 10-30M requests/day or >$20k/month in API
spend - nowhere close to a 2-3 person CRM's usage, and would need Emile to
buy/rent a real GPU and keep a server running 24/7 for it. Advised clearly
against self-hosting for his situation: the Gemini flash-lite fix above is
the right answer, not a stepping stone - self-hosting would cost more, not
less, at this scale. No action taken, purely advisory; worth remembering if
this comes up again so it isn't re-researched from scratch.

## Round 13 — full quality/bug audit across the whole app (2026-08-31)

Emile asked for a full pass before considering this "one of the last
steps" — UI, loading time, usability, and a real bug hunt across
everything, "take your time." Approach: 3 parallel read-only Explore agents
covering the whole app in sections (Pipeline/Contacts/Digest/Reporting;
Conversations/Discussion/Calendar/Map; Documents/Team/Training/Settings/
shared layout), plus a live browser walkthrough (console errors, network
requests, real interactions) done directly, not delegated. All actual fixes
applied directly per [[feedback/avoid-subagents-for-hands-on-builds]] — only
the investigation phase used agents.

**Real bugs found and fixed (verified via tsc/eslint/`next build` clean,
plus live browser re-testing of the highest-impact ones):**
- **Pipeline board went stale** (`kanban-board.tsx`) — local state was
  seeded once from props and never resynced, so `router.refresh()` after a
  move (or any change made elsewhere - a teammate moving a card, a new lead
  arriving) was silently ignored until a hard reload. Fixed using React's
  render-time state-adjustment pattern (not a `useEffect`, which this
  project's own lint rule flags for this exact case, same class of issue as
  the `event-dialog.tsx` reset bug from Round 5). Verified live: dragged a
  second lead, no regression.
- **Calendar `updateEvent`** ran 3 sequential un-transactioned DB calls
  (delete attendees, delete leads, update) — a mid-failure could wipe an
  event's attendees/leads with the update never applied. Wrapped in
  `db.$transaction`.
- **Calendar event delete had no confirmation** (destructive, one misclick)
  — added, matching the pattern Map's route-line delete already used.
  Calendar save also had a silent-failure gap — added try/catch+toast.
- **Map's "Enregistrer" button could get stuck forever** on a save error (no
  try/catch, so `setSaving(false)` never ran on failure) — fixed with
  try/catch/finally. Delete-line also silently swallowed permission errors —
  fixed.
- **Map page over-fetched** every contact column just to render pins that
  need 5 fields — added a `select`.
- **Discussion's `listMessages` was unbounded** and polled every 5 seconds
  forever — a real, compounding cost as history grows. Capped at the most
  recent 200. Also fixed an N+1 write (`sendMessage` looped `await` per
  tagged contact instead of `createMany`), and added missing try/catch to
  three action handlers (`openDm`, `handleCreateGroup`, `handleSend`) that
  were silently swallowing failures.
- **Conversations page fetched every contact's full activity history**
  (no cap) on every load — capped at 100/contact. Also fixed a missing
  try/catch on the CALL/NOTE compose branch (every other channel already had
  one).
- **Contacts: silent save failures** — `contact-detail-form.tsx` and both
  quote/contract generation dialogs (`document-generator.tsx`) had no
  try/catch, so a failed save or PDF generation looked like nothing
  happened, no toast, no error. Fixed all three.
- **Settings: broken auto-reply toggle for non-owners** — the card rendered
  for everyone but the underlying action is OWNER-only server-side; a
  non-owner's click flipped the checkbox UI immediately with no `await`/
  `.catch()`, so it silently failed and looked broken until next reload.
  Fixed: disabled for non-owners (with an explanatory `title`, matching how
  `TeamInvites` handles the same class of gap), and the actual owner path
  now awaits + reverts + toasts on a real failure. Verified live: toggled
  on/off as owner, both calls succeeded cleanly.
- **Team role labels were inconsistent across 3 places** — `lib/roles.ts`'s
  `ROLE_LABEL` ("Vente / Porte-à-porte") vs. hardcoded duplicates in the
  sidebar and the invite dialog ("Vente") giving different French wording
  for the same role depending on which screen you're on. Unified to the one
  source of truth.
- **Security hardening:** `Content-Disposition` filename headers on both
  document-download routes interpolated a contact's editable name directly
  - a name containing a `"` or newline could produce a malformed header and
  crash the download. Added a shared `sanitizeHeaderFilename` helper.
- **My own bug from Round 11, caught in this pass:** the new help chat
  widget's `bottom-4 right-4` position visually collided with Discussion's
  (and potentially Conversations') message-composer send button, confirmed
  via real DOM rect overlap - bumped to `bottom-20` so it clears any
  page-level bottom-right control. Also added missing `aria-label`s to the
  widget's own close/send buttons (an agent caught this reviewing my own
  code).
- Minor accessibility: digest's prev/next day buttons and training's delete
  button had no accessible label - both fixed. Training delete also had no
  confirmation - added. Removed one piece of genuinely dead code (a
  redundant `sr-only` initials span in `contact-card.tsx`).

**Flagged but deliberately NOT changed - product/security policy
decisions, not bugs to silently fix:**
- `deleteDocument` (documents.tsx) and both training-resource actions
  (training.ts) have **no auth/role check at all** - any logged-in user,
  any role, can delete any document or any other person's training
  material. May be intentional ("anyone can curate/manage"), may not be -
  worth a deliberate decision from Emile, not something to silently
  restrict.
- Calendar's `Event.allDay` schema field exists but the create/edit form
  always hardcodes `false` with no UI control - a dead, unreachable field.
  Left as-is pending a decision on whether all-day events are wanted.
- Contacts: quote numbering is one global sequential counter with no
  reservation - a real but very-low-probability race if two people generate
  a quote at the exact same moment. Noted, not fixed (low severity, small
  team).

**Not yet browser-verified individually:** the Calendar delete-confirmation
and silent-failure fixes (no events existed in the current date range to
test against without creating throwaway data) - verified via clean
tsc/eslint/build and by the identical pattern already proven working
elsewhere (Map's line-delete confirm), but not clicked through live like
the pipeline/settings/discussion fixes were.

Verified end-to-end via a **fresh, never-before-opened browser tab**
(important - the long-lived dev tab's console had accumulated stale errors
from earlier in the session, including from mid-edit HMR churn, that looked
alarming out of context but weren't live bugs): all 12 top-level pages
return clean 200s, zero console errors on a cold load. `tsc`/`eslint`/
`next build` clean on the full tree throughout.

## Round 14 — daily-snapshot caching for Digest/Reporting (built + verified 2026-08-31)

Emile reported the Round 12 perf fix wasn't enough in practice: even at
1.4-2.3s per load, /digest and /reporting recomputing from scratch on every
single page open still felt slow/wasteful to him. **Requested design change,
his explicit spec:** stop recomputing on every page load entirely. Instead,
generate the digest/report **once per day at a fixed cutoff time (he
suggested 6 PM)** and serve that same static snapshot to everyone, all day,
regardless of how many times the page is opened or by whom — a genuinely
shared team artifact, not a per-request or per-user computation. Next
calendar day's snapshot only regenerates after the next 6 PM cutoff. His own
words: "I don't care how it works, I just want one every single day and it
works fine" — implementation approach is delegated, the requirement is the
fixed daily-artifact behavior. Applies to both /digest and /reporting.

**Built and verified same day.** New `DailySnapshot` Prisma model (kind +
YYYY-MM-DD business date + JSON payload, unique on both), a dependency-free
`src/lib/business-day.ts` computing the "effective business day" from real
Eastern time via built-in `Intl` (before 6PM = still yesterday's business
day, at/after 6PM = today's — no new npm package needed for this). Both
pages now call a `getDailyDigest()`/`getReportingSnapshot()` wrapper:
read the cache row for the resolved date, else compute + store + return.
Past digest days (existing `?date=` browsing) are cached forever on first
view since they're closed and immutable. Also added, per Emile's own
follow-up choice when asked: an owner-only "Régénérer" button on both pages
(mirrors the existing ads "Sync now" pattern) that clears just the current
day's cache row as a manual escape hatch.

Schema applied via `prisma db push` (not `migrate dev`, which wanted a
full destructive reset over pre-existing unrelated drift — same recurring
situation as Round 6's Meta schema change). Verified for real against the
running dev server, not just typechecked: confirmed actual `DailySnapshot`
rows appear in the real SQLite file after a cache-miss load, a second load
of the same page is a fast cache-hit (~100ms vs. 2-4s cold) with zero new
GA4/Gemini calls fired, past-date digest browsing (`?date=2026-08-20`)
renders correctly and caches independently, the "Régénérer" button actually
clears the row and forces a fresh compute on next load (confirmed via
`createdAt` changing), and — logged in as a temporary SALES-role test
user — the button is correctly hidden for non-owners. `tsc`/`eslint`/
`next build` all clean.

**One real gotcha hit again this round, consistent with the Round 4 note:**
the dev server needs a full restart after `prisma generate` picks up a new
model, or the old cached Prisma client throws `Cannot read properties of
undefined (reading 'findUnique')` on the new table.

**Known limitation, told to Emile directly:** since no cron/scheduler infra
exists yet (the app is still local-only), the very first person to open
either page after a day's 6PM cutoff still pays the original cold-compute
cost (~2-4s) — everyone after them gets the instant cached version. A
scheduled pre-warm job would remove even that, but needs real hosting
(Netlify/Vercel) with a scheduler first, which hasn't happened yet.

## Round 15 — ad comparison + real per-post social analytics (built + verified 2026-08-31)

Emile asked two related questions: whether the CRM could help analyze social
posts to get more views/likes, and whether two ad campaigns can be compared
side by side. Explored first (no per-post Meta data exists today - only
Page-level aggregate engagement; ad campaign data already exists per-day
per-campaign but has never been surfaced as a comparison view).

**Confirmed decisions (AskUserQuestion, 2026-08-31):**
- **Post analytics: real per-post data, not just aggregate.** Emile chose
  to pursue actual per-post Instagram/Facebook metrics (likes/reach per
  post) over a simpler v1 built on existing aggregate data, explicitly
  accepting that Instagram per-post insights need a new Meta permission
  (`instagram_manage_insights`) and likely weeks of Meta App Review - the
  same kind of wait as the still-blocked Instagram DM messaging feature
  from Round 6. Facebook Page post-level insights likely already work with
  the currently-granted `pages_read_engagement`, so that half can be built
  and verified now while Instagram-specific per-post data waits on the new
  permission/review, same graceful-degradation pattern as other
  pending-Meta-review features.
- **Ad comparison: pick any 2 campaigns, side by side** (not an extension
  of the existing flat "Détail par campagne" table) - dropdowns to select
  two specific campaigns, their key metrics and trend lines shown next to
  each other over a selectable date range. All underlying data
  (`computeCampaignPerformance()` in `src/lib/ads-metrics.ts`, daily
  `AdSpendDaily` rows per campaign) already exists; this is purely a new UI
  surface, no new data plumbing needed for the Meta/Google campaign side.

**Built and verified same day.**

**Feature A (ad comparison):** new `computeCampaignSeries()` in
`src/lib/ads-metrics.ts` (per-campaign day-by-day series, alongside the
existing all-time `computeCampaignPerformance()`), baked into the existing
`/reporting` daily-snapshot cache (`meta.campaignSeries`, no new Prisma
model). New "Comparaison" tab: two campaign-picker dropdowns + a 7/30/90-day
range selector, side-by-side stat tiles and a dual-line Recharts trend
chart, fully client-side (no server round-trip per pick). Verified live
with a temporary second test campaign (inserted and cleaned up via direct
SQL) since only 1 real campaign exists in the live data right now: numbers
matched the existing "Détail par campagne" table exactly, chart rendered
correctly, guard state ("Choisis deux campagnes à comparer") worked.

**Feature B (social post analytics) - real, important finding from live
testing, not just planned behavior:** built `src/lib/meta-posts.ts` (lists
Facebook Page posts + Instagram media, attempts per-post insights for
both), `SocialPost` Prisma model, `/social` page (new nav entry "Réseaux
sociaux"), Gemini-powered recommendations reusing the existing AI plumbing,
and a new Settings card. Ran the real "Synchroniser maintenant" sync against
Emile's actual connected Meta account (not mocked): **post LISTING works
for both platforms already** (1 Facebook post, 18 Instagram posts pulled
in for real), but **the actual metrics (likes/comments/reach/etc.) are
blocked for BOTH platforms right now, not just Instagram as originally
assumed during planning.** Facebook's per-post numbers need Meta's "Page
Public Content Access" feature (a separate Advanced Access grant, not
covered by the already-checked `pages_read_engagement` permission) -
confirmed via the real API error: "(#10) This endpoint requires the
'pages_read_engagement' permission or the 'Page Public Content Access'
feature." Instagram needs `instagram_manage_insights` as expected. Fixed
the code to detect this correctly (post listing succeeding does NOT mean
metrics are available - checks per-item insight-call failures, not just
the top-level list call) and to stop the AI recommendations from drawing
conclusions off fake all-zero data (caught a real early output where
Gemini said "posting late doesn't work" based on zeros that were actually
just blocked-metric placeholders, not real engagement - added an explicit
check that returns an honest "Meta hasn't given us real numbers yet"
message instead whenever every post's metrics are 0). Settings card
updated to give Emile accurate two-track setup steps (Page Public Content
Access via App Review for Facebook, `instagram_manage_insights` for
Instagram), both realistically multi-week waits.

**Also fixed a real house-style bug on my own part:** wrote an em dash into
one of the new fallback/toast strings despite knowing [[feedback/no-em-dashes]]
- caught it during verification (visible in the live rendered page) and
fixed all instances across the new files before calling this done. Worth
remembering: even code that isn't "writing to Emile" directly (UI copy,
toast messages) is still user-facing text and subject to the same rule -
only internal code comments are exempt.

`tsc`/`eslint`/`next build` all clean throughout. Both features verified
against the real running dev server and Emile's real connected Meta
account, not just typechecked.

## Round 16 — AI trustworthiness pass across the app (built + verified 2026-08-31)

Emile raised a general worry, not tied to one feature: he doesn't know if
the AI suggestions across the CRM (starting with the daily digest) are
actually good advice or "just another AI slot," and doesn't want to act on
something that could downgrade the business. Asked whether the fix is
feeding the AI more context about how ads/the website work.

**Audited every real Gemini call site first** (not guessed): found
`src/lib/digest-trends.ts`'s `phraseSignals()` was already the gold
standard in this codebase - explicit "never invent or change the given
numbers" instruction, a real week-over-week baseline
(`leadVolumeSignal`), and a deterministic non-AI fallback always
available. `src/lib/social-recommendations.ts` (built the round before)
was the one place doing real business analysis without that same rigor -
no anti-invention wording, no baseline to compare against, no requirement
to cite which specific post backs a claim. `help-chat.ts` (navigation
help, not business analysis) and `ai-draft.ts` (generative drafting Emile
reviews before sending) were confirmed fine as-is, different risk
profiles. Confirmed via grep: **no industry-benchmark data exists
anywhere in the codebase** - decided against fabricating external
"industry-typical CTR/CPL" numbers (that would itself be a hallucination
risk) in favor of the same real internal-baseline pattern
`leadVolumeSignal` already proved: compare the business against its own
recent history, not an invented outside reference point. Emile confirmed
via AskUserQuestion: stay on the free `gemini-3.5-flash-lite` model for
now, this prompt/data work is the real lever - revisit the model only if
output still feels weak after this fix.

**Built:** `src/lib/social.ts` now computes a 30-day-vs-prior-30-day
engagement baseline (same two-window shape as `leadVolumeSignal`) and
passes it into `generateContentRecommendations`.
`social-recommendations.ts`'s `SYSTEM_PROMPT` now carries the exact
"sans jamais inventer ou changer les chiffres donnés" wording copied
verbatim from `phraseSignals()`, plus a new requirement to cite the exact
publication date behind every recommendation.

**Real bug caught and fixed during verification, not just assumed
working:** the per-post prompt lines only included day-of-week + hour, not
the actual calendar date, so the model cited things like "la vidéo du
samedi" (which day-of-week, no way to trace back to a real row) instead of
a real date matching what the `/social` post table actually displays.
Verified end-to-end with a standalone `tsx` script (synthetic in-memory
post data, since Meta's real per-post metrics are still all zero/blocked
- confirmed this correctly falls back to the honest "no real numbers yet"
message rather than running the AI on fake zeros) run twice: before the
date fix the output cited "la vidéo du samedi à 21h" (untraceable); after
adding the real date to the prompt data, the output cited
"ta publication du 2026-08-29" and "celle du 2026-07-22" - both real,
traceable, and numerically accurate (56 vs 11 engagement, matching the
synthetic input exactly). `tsc`/`eslint`/`next build` clean, `/digest`
confirmed unaffected (no changes made there, it was already correct).

## Round 17 — minimum-sample guard + real business context for AI (built + verified 2026-09-01)

Emile asked to keep going on AI trustworthiness: wanted a minimum-sample
guard for social recommendations (agreed to from the options presented),
and separately asked to "give it basically everything about our
business" so recommendations don't sound smart but be operationally wrong
(e.g. suggesting winter outdoor work). Confirmed his framing wasn't a
literal data dump - narrowed to durable operational facts before writing
anything, since baking stale/wrong facts into an AI prompt would be the
exact failure mode this whole effort is trying to prevent.

**Confirmed/corrected directly with Emile before writing code (not
assumed from memory alone):** the "2-person operation" framing needed
softening - sales reps and a cleaning crew are planned for the 2027
season, not yet real, so phrased as current-state-with-a-caveat rather
than a fixed fact. Real lead channels right now, confirmed directly:
**Meta ads, the website form, door-to-door, and some word of mouth -
explicitly NOT Google Ads** (that campaign exists in the CRM's
code/schema but was deliberately never launched for 2026, per
[[duo-vert/google-ads-campaign]]) - a fact the AI could easily have
gotten backwards from the schema alone without this correction.

**Built:** new `src/lib/business-context.ts` - a single exported
`DUO_VERT_CONTEXT` constant (one dense French paragraph: services,
how the work is done, season, pricing model, service area, real lead
channels) as the single source of truth, prepended into both
`digest-trends.ts`'s `phraseSignals()` prompt and
`social-recommendations.ts`'s `SYSTEM_PROMPT` - not duplicated per file.
`ai-draft.ts`/`help-chat.ts` stayed out of scope again, same reasoning as
Round 16. Added `SOCIAL_MIN_SAMPLE = 3` to `social-recommendations.ts`
(matches `digest-trends.ts`'s `LEAD_VOLUME_MIN_SAMPLE` naming/threshold
convention) - below 3 metric-bearing posts, returns an honest
"pas assez de publications pour dégager une vraie tendance" message
instead of calling Gemini.

**Verified via the same standalone `tsx`-script approach as Round 16**
(synthetic in-memory data, since Meta's real per-post metrics are still
blocked): confirmed the min-sample guard fires correctly at 2 posts, and
with 3 posts the AI output visibly used the business context - it named
Emile and Beckett's actual pressure-washing equipment unprompted and
recommended more on-the-job video content, not generic social media
advice. Live-verified on the real running app too: `/digest`'s lead-drop
recommendation now says to push door-to-door/Meta specifically (the real
channels), not a generic "increase marketing" line; `/social` still
correctly shows the honest zero-metrics fallback since real Facebook/
Instagram numbers are still pending Meta approval, unaffected by this
round's changes. `tsc`/`eslint`/`next build` clean throughout, and the
em-dash check happened proactively this round (checked all new prompt
text before running any tool, per the Round 16 lesson) instead of being
caught after the fact.

**Round 18 (2026-09-01)** - "seen/read" tracking across all three outbound
channels, prompted by an open-ended "find something useful in the CRM"
ask that Emile then expanded to cover documents, email, and SMS with real
timestamps, not just a seen flag:
- **Documents (quote + contract):** `Document.viewedAt` (first-view only,
  same nullable-timestamp pattern as `quoteSentAt`), tracked in the public
  PDF-byte route (`api/public-documents/[token]`, not the page route -
  keeps link-preview crawlers from producing false positives by
  construction), logs a `DOCUMENT_VIEWED` Activity. Fixed a real trap:
  `staleLeadsSignal` was treating any activity (including this new passive
  one) as "not stale," which would have silently hidden a gone-quiet lead
  right when it should surface. Added a sibling digest signal,
  `quoteViewedNoFollowUpSignal` (2-day threshold vs. the 5-day generic
  one), rendered as its own "hot leads" block above the generic stale-lead
  list.
- **Email:** Gmail sends were text-only until now - switched to
  multipart (text + HTML) so a tracking pixel has somewhere to live.
  New `Activity.readAt`, set via a public pixel route
  (`api/track/email/[token]`) keyed by the Activity's own id (created
  before send, rolled back if the send fails). Documented the real,
  known caveat: pixel tracking can log false-early reads if the email
  client proxies images - same industry-standard tradeoff every CRM in
  this space makes, not hidden from Emile.
- **SMS:** genuinely blocked on two things outside the code (Sent unfunded,
  no public hosted URL for a webhook), so built the receiver
  (`api/webhooks/sent`) and signature verification (real HMAC-SHA256
  scheme, fetched and confirmed directly from docs.sent.dm rather than
  guessed) code-ready but inert until both blockers clear.
- Schema pushed via `prisma db push` (not `migrate dev`, which wanted to
  reset the whole dev database over unrelated drift already present -
  refused, used the non-destructive path instead).
- Verified for real: `tsc --noEmit` and `next build` both clean. Live
  runtime check via a one-off standalone script (bypassing another
  session's dev server, which held a stale pre-regeneration Prisma client
  in memory - a real, self-resolving artifact of two sessions sharing one
  process, not a bug) confirmed against the real dev database: view/read
  tracking fires once and no-ops on repeats, bot user-agents are filtered,
  a tampered webhook signature is correctly rejected, a valid one is
  correctly accepted. All test data reverted after - the real Duo Vert
  document/contact rows touched during verification were left exactly as
  found.

**Spending decision (2026-09-01):** Emile decided not to pay for or
activate anything until it will actually be used live. Concretely for the
CRM: proceed with the free Google Cloud OAuth app setup (needed for Gmail
integration, no cost) and continue feature/optimization work, but do NOT
fund the Sent account for SMS, and do NOT stand up any paid hosting tier
(Turso) until ready to actually run it. Same logic applies to the AI phone
receptionist idea from [[personal/agency-idea]] - fine to prototype/wire
up structurally, not to pay for the parts that cost money (telephony/
voice model usage) until it's actually going to be used. See
[[feedback/no-paid-setup-before-ready-to-use]] for the general version of
this rule.

**Full monthly operating cost estimated (2026-09-01), consolidated from
scattered figures into one total; corrected same day after Emile pushed
on whether hosting could be genuinely free:** core hosting -
Netlify (app, free) + Turso (database) + Cloudflare R2 (document
storage) = **$0/month realistically, likely indefinitely** at Duo Vert's
actual 2-3 person scale - Turso's $4.99/mo and R2's paid tier are ceilings
that only kick in if the business outgrows their free tiers, not an
expected cost. **Real free-tier limits, checked directly (2026-09-01):**
Turso - 500 million row reads/month, 10 million row writes/month, 5GB
storage; Cloudflare R2 - 10GB storage, 1 million writes/month, 10 million
reads/month, $0 egress fees always. Translated to real usage: Duo Vert's
CRM traffic is thousands of reads/day at most (nowhere near 500M/month),
and quote/contract PDFs (50-500KB each, a few dozen/month) would take
tens of thousands of documents to fill 10GB. Realistic conclusion: paid
tier isn't a near-term concern at all, not this year or likely ever
unless the business grows into real daily-active-user counts in the
dozens-to-hundreds range. Considered and rejected: self-hosting on a traditional VPS
to avoid these entirely - not actually free (~$5-6/mo server rental) and
adds real maintenance burden (security updates, backups, uptime) the
managed free tiers currently avoid - not a good trade. **Gemini** (AI
drafting/digest) also runs on a free API key, same "unlikely to become a
real cost at this volume" logic. **Confirmed with Emile: the only two
things that cost real money are SMS (Twilio) and the AI phone
assistant**, because those are the two pieces paying for actual outside
infrastructure (carrier networks, per-minute voice processing) rather
than just running the app. On top of the
near-$0 hosting, if/when turned on: SMS/texting (Twilio, ~$6-8/mo) + AI
phone assistant (Vapi, ~$10-40/mo, see [[personal/agency-idea]] for the
full build-out). **Grand total for genuinely everything: ~$16-48/month,
most realistically ~$20-30/month** - almost entirely the messaging/
calling layer, not the app/hosting itself.
