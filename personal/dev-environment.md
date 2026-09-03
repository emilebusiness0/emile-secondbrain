---
name: personal-dev-environment
description: What's installed and authenticated on Emile's Mac for Claude Code to use directly, so future sessions don't assume tools are missing
metadata:
  type: user
  modified: 2026-09-02
---

**Homebrew**: installed 2026-08-01 (`/opt/homebrew/bin/brew`). Not yet on PATH in already-open Terminal windows/sessions — new windows pick it up automatically via `.zprofile`; from inside a Claude Code session, call it by full path (`/opt/homebrew/bin/brew`, `/opt/homebrew/bin/gh`) rather than assuming `brew`/`gh` resolve on PATH.

**GitHub CLI (`gh`)**: installed via Homebrew 2026-08-01, authenticated as `emilebusiness0` over HTTPS with scopes `gist, read:org, repo, workflow`. This means repo-level admin actions (rename, create, delete, PR/issue management) can be done directly via `gh` from Claude Code — no more walking Emile through the GitHub Settings UI manually for things `gh` can do. Reason it got installed: hit a wall renaming the `second-brain` → `emile-secondbrain` GitHub repo (see [[feedback/rename-move-verification-checklist]]) with no CLI available; had to ask Emile to do it via browser.

**Existing git auth (separate from `gh`):** `git config credential.helper` returns `osxkeychain` — a cached credential already lets plain `git push`/`pull` work without prompts, independent of `gh`'s own auth. Both now work; `gh` adds account/repo-admin-level actions on top of what plain git already could do.

**Obsidian community plugins (2026-08-01):** Emile browsed the community plugin store and screenshotted ~20 options. Claude only gave a reasoned opinion on 5 of them: recommended **Smart Connections** (local semantic search/related-notes, no API key) as worth considering, noted **Tag Wrangler**/**Linter** as nice-to-have but not urgent, and advised skipping **Copilot**/**Claudian** (redundant with the REST API MCP already giving Claude direct vault access — see [[duo-vert/memory-architecture]] — Claudian is also explicitly unreviewed by Obsidian staff). The other ~15 plugins shown (Excalidraw, Templater, Dataview, Tasks, Git, Calendar, Kanban, Iconize, Remotely Save, QuickAdd, etc.) were never evaluated by Claude at all. Emile installed only Smart Connections and judged the rest not worth it himself — that's his own call, not a Claude assessment, so don't treat this as "Claude reviewed and rejected 20 plugins."

**Playwright MCP (added 2026-08-02):** `claude mcp add playwright npx @playwright/mcp@latest` — launches its own separate Chromium instance (not Emile's real Chrome), keeps a persistent browser profile/cookies across sessions once logged in once. Added specifically to view logged-in Google properties (GSC, GA4, Netlify) for Duo Vert without the Chrome-profile/account conflicts described below. **Important limitation: MCP server connections are per-session** — a server added or connected in one Claude Code session is NOT retroactively available in a conversation that was already running before the add/connect happened. It only shows up in sessions started (or reconnected) after the server is live. If a fresh session needs Playwright, just ask directly — no special setup needed beyond it being registered globally via the command above.

**Claude in Chrome extension — known blocker, unresolved:** Emile's Claude account (personal, paid) is separate from the Google accounts used for Duo Vert's Netlify/GSC/GA4 (a `duovert` Google account). The extension only cares which *Claude* account it's signed into (not the Google account of open tabs), but Chrome enforces one active Google session per profile, so switching between "personal Claude login" and "duovert Google login" in the same Chrome profile causes conflicts. Never fully resolved — Playwright MCP (above) was set up as the working alternative instead, since it doesn't have this constraint (its own isolated browser + profile). Don't recommend Claude in Chrome for Duo Vert browser tasks unless this gets sorted out.

**Sapphire notch app (installed 2026-08-15):** free/open-source (github.com/cshariq/Sapphire), installed via .pkg release, lives in /Applications. Turns the MacBook notch into a drag-and-drop file shelf, plus now-playing music widget with controls, window snapping, and AirDrop-near-notch. Chosen over paid alternatives (Dropover) and single-purpose ones (NotchDrop, NotchBox) specifically because it's free and bundles more features.

**Sapphire password-typed-into-active-app bug — likely cause found and resolution in progress (2026-08-17):** Emile noticed his Mac login password getting typed as plaintext into whatever app had focus (e.g. Google Sheets) after the screen dimmed/went black from inactivity. He was confident it was Sapphire and initially believed it typed itself (not him), suspecting a "keep awake because my phone is nearby" mechanism. Root cause found via Sapphire's README: it has a **Lock Screen** feature that draws its own widget overlay (weather/music/calendar) over the real macOS lock screen — most likely explanation is that overlay steals focus from the actual password field, so keystrokes typed expecting the real prompt pass through to the app underneath instead. A separate **Caffeinate** feature exists (manual button, not automatic on phone proximity) — probably unrelated. **Emile's decision: keep Sapphire installed, disable its Lock Screen widget toggle specifically in Sapphire's own Settings, and does NOT want to change his password** — he's confident nothing was exfiltrated. Emile reports (2026-08-17) he believes it's fixed after disabling the toggle — not yet battle-tested over many cycles, so still worth asking if it recurs.

**MCP tools connected for CRM-adjacent work (noted 2026-08-17):** Gmail (send/reply/draft/label/search threads), Google Calendar (create/find/update events, availability), Google Drive, and a messaging service called "Sent" for SMS/WhatsApp/RCS (contacts, message history, templates, balance) — haven't yet checked what's actually configured on Sent (number, balance, templates). Relevant context: came up when Emile asked whether I could build a free GoHighLevel-style CRM — see [[duo-vert/sheets-tracking]] for that discussion. These tools mean email/SMS/calendar aren't hypothetical builds, they're already live integrations, but I'm not an always-on background listener — I only act when a session is open.

**In-app "Claude Browser" pane (Claude_Browser MCP) — not signed into Emile's Google account (confirmed 2026-08-18):** opening a Google Doc/Sheet in this browser pane shows "Request edit access / Sign in" — it's an unauthenticated session, separate from both Playwright MCP's persistent profile and Claude in Chrome. Cannot be used to type/edit into Google Docs/Sheets unless Emile signs in within that pane first. Discovered while the Drive connector's `create_file`/`copy_file` tools were both erroring ("Tool execution failed") for an unrelated reason — don't assume the browser pane is a fallback path for Drive edits without checking sign-in state first.

**CRITICAL — Gmail MCP `create_draft` actually sent an email instead of drafting it (2026-08-18).** Called `create_draft` (to Lisette, with the quote PDF attached) and reported it to Emile as "sitting in Drafts, not sent." It was not. Verified afterward via `get_thread`/`search_threads` that the message carries a `SENT` label and is findable with `in:sent`, and it had disappeared from `list_drafts` entirely. So the tool (or something downstream of it on this account) actually delivered the email rather than just creating a draft — the content happened to be correct/approved in this case, but the send itself was never confirmed by Emile, violating the explicit-permission-required rule for sending messages on his behalf. **Root cause not yet identified** (tool bug vs. an account-level auto-send rule). **Until this is understood: after every `create_draft` call, immediately verify with `list_drafts` (or `search_threads` with `in:draft`) that the item is actually sitting in Drafts — and separately confirm it's NOT in Sent — before telling Emile it's safe/unsent. Do not trust the tool's own success response as proof nothing was sent.**

**Follow-up same-incident findings (2026-08-18, still same day):** (1) The PDF attachment on that auto-sent email rendered corrupted when Lisette opened it in Gmail's viewer — raw PDF content-stream operators leaking as visible text partway down page 1 (e.g. "...totioa main-d'œuvre2336,47$T* ET Q Q q 1 0 0 1 70.8 366 cm..."). Diagnosed by installing `qpdf`/`poppler` (`brew install qpdf poppler`) and running `qpdf --check` + `pdftotext -layout` against the local source file — both came back completely clean, and manually round-tripping the exact base64 blob I'd used (`base64 -d`) reproduced a byte-identical, valid PDF. So the source file and the base64 encoding of it were never broken; the corruption happened somewhere in getting that ~5000-char base64 string into the `create_draft` tool call itself, and there is no tool available on this connector to download a sent message's actual attachment bytes back for direct comparison, so the exact point of corruption couldn't be pinned down further. (2) A **second, independent stray draft** was discovered via `list_drafts` — from an earlier `create_draft` call that had returned an outright error ("The service is currently unavailable") — meaning that failed call had *still* created a partial empty-body draft on Gmail's side despite reporting failure to me. **Combined lesson: don't trust this connector's success/error responses as ground truth for anything (sent vs. draft, created vs. failed) — always independently verify via `list_drafts`/`search_threads`/`get_thread` afterward, and check for stray partial drafts left behind by calls that appeared to fail.** For attachments specifically, treat any base64 payload above a trivial size as a corruption risk on this connector until a way to verify the round-trip end-to-end (not just locally) is found.

**Prisma's AI-safety gate and Claude Code's auto-mode classifier are two independent blocks (2026-08-29):** running a destructive-looking Prisma command (`db push --accept-data-loss` on a real dev database with actual schema drift) got blocked twice in a row for different reasons — first by Prisma itself (its CLI detects an AI agent and refuses without a `PRISMA_USER_CONSENT_FOR_DANGEROUS_AI_ACTION` env var carrying Emile's literal consent text), then again by Claude Code's own auto-mode permission classifier even after that env var was supplied correctly. Satisfying Prisma's own gate does not satisfy the harness's separate one — getting past both needed Emile to explicitly switch the session out of auto mode. Worth remembering if a similarly "dangerous" CLI command (migrations, resets, force-pushes) gets blocked again: check whether it's Prisma-level, harness-level, or both, rather than assuming one fix covers it. Full incident: [[duo-vert/custom-crm-prototype]] Round 6.

**No ffmpeg/exiftool installed, and Read tool can't open video files directly (found 2026-09-01):** when asked to review `.MOV` files (Beckett's job-site videos for GBP post drafting), the Read tool errored with "cannot read binary files" on a `.mov`, and `ffmpeg`/`ffprobe`/`exiftool` were all absent from PATH. There is no way to inspect video content (extract a frame, read metadata) until one of these is installed via Homebrew — install is a real system action, so in plan mode it needs explicit approval first, it can't just happen as part of research.

**Playwright MCP has live, authenticated access to Duo Vert's actual Google Business Profile (confirmed 2026-09-01):** navigating to `business.google.com/posts` (or really any Google search for "Duo Vert" while signed into the `duovert` account) lands on the Google/Maps "Business Profile on Search" panel, already authenticated as `duo.vert.gatineau@gmail.com` with "You manage this Business Profile" showing. This is the actual live GBP management surface — Posts, Photos, Reviews, Edit profile are all directly clickable and functional through Playwright, no separate business.google.com login needed. The "Add post" composer (via the "Posts" button or "Add update") has a native **"Schedule this post"** toggle with Date and Time fields, meaning posts can be scheduled for a real future date/time in one session rather than needing to return and post manually on the day. Date field format is MM/DD/YYYY regardless of the `hl=en-GB` in the URL; always confirm via the "Open calendar" picker (it shows the resolved day/month/year and marks the selected date) rather than trusting the typed text field, since a misread date would publish content on the wrong day on a live public listing. This capability was used to schedule a pilot batch, extended to 20 GBP posts for Duo Vert (all confirmed "Scheduled," none rejected). **Video posts show a transient "Pending" status right after upload** (vs. "Scheduled [date]" for photos) — this resolved to "Scheduled" within a couple minutes in both cases tested, so it's normal processing, not a rejection signal. Don't mistake "Pending" on a freshly-uploaded video for a problem; just re-check the posts list a little later.

**Claude plan: currently Pro, expects to need Max soon (2026-09-01).**
Emile asked directly whether his growing agency work (multiple client
sites/CRMs, heavy daily Claude Code use, see [[personal/agency-idea]])
will require upgrading from Pro. Real numbers checked: Pro gives ~10-45
Claude Code prompts per 5-hour window; Max 5x ($100/mo) gives ~225/window,
Max 20x ($200/mo) gives ~900/window - all plans share a weekly cap across
Claude Code, chat, and sessions like this one. Assessed as likely needed
soon, not hypothetical - a single feature-build session (schema changes,
multiple files, live verification, research passes, like Round 18 in
[[duo-vert/custom-crm-prototype]]) is exactly the kind of multi-file
agentic work that burns through Pro's limit fast, and this will compound
once he's juggling multiple client projects at once. Recommended
upgrading reactively (once actually hitting rate limits regularly,
expected within a month or two given his stated plans) rather than
preemptively - same "don't pay before you need it" logic already applied
elsewhere (see [[feedback/no-paid-setup-before-ready-to-use]]). Emile
explicitly said he doesn't mind the cost given what Claude has already
made him.

**Correction, same conversation:** Emile clarified he's currently
self-rationing to Sonnet 5 on medium effort specifically to conserve his
Pro limits, and rarely hits his 5-hour cap, never hit the weekly one.
This changes the calculus from the note above - he's not currently
bottlenecked, so upgrading now wouldn't remove a blocker, it would buy
two different things: defaulting to Opus instead of self-rationing to
Sonnet, and running multiple sessions in parallel. Framed to him as a
preference decision (worth $200/mo now for better models/parallelism) vs.
a necessity decision (upgrade once actually rate-limited) - he hasn't
picked one yet, but confirmed being fine with the cost either way.

**Clarified tier confusion, same conversation:** Emile thought there was
a big gap between Pro (~$24 CAD/mo) and "Max" at a single $140 CAD/mo
price, with no middle option. Resolved: Max is two separate tiers, not
one - Max 5x (~$140 CAD/mo ≈ $100 USD, 5x Pro's usage) and Max 20x
(~$270-280 CAD/mo ≈ $200 USD, 20x usage). The $140 figure he'd seen was
already Max 5x, not a single monolithic "Max" plan. Given he estimated
needing roughly 2-3x his current usage, **Max 5x is a good match** - not
overkill the way Max 20x would be. No actual gap in the plan lineup for
his stated need, he just hadn't realized Max splits into two tiers.

**Team plan checked and ruled out for now, same conversation:** Team
Standard is $20-25/seat/mo but only 1.25x Pro's usage (not worth it).
Team Premium is $100-125/seat/mo, 6.25x usage (comparable to Max 5x) -
but Team plans require a **minimum of 2 seats**, so solo it would cost
~$200-250/mo for roughly what Max 5x gives one person for ~$140 CAD/mo
alone. Conclusion: **Max 5x remains the right call for his actual
situation** (solo, agency work). The one scenario where Team could make
sense - splitting a Premium plan with Beckett - is only worth pursuing if
Beckett would genuinely use Claude Code himself; flagged as something to
actually ask him rather than assume, since Beckett's role has been more
social/sales than technical build work (see
[[duo-vert/season-2027-plan]]).

**LibreOffice installed 2026-09-02, and Office-file tooling gaps found on this Mac:** while building a Word deliverable for a Cégep assignment (see [[cegep-school-organization]]), discovered this Mac had none of the tools the docx/pptx skills assume: no `markitdown`, no `pandoc`, no `python-docx`/`python-pptx`, and no LibreOffice (`soffice`) at all — so reading/converting/rendering Office files needed workarounds (parsing docx/pptx XML directly with Python's stdlib for reading) until `brew install --cask libreoffice` was run to get `soffice`/`pdftoppm`-based visual QA working. Also: the `docx` and `pptxgenjs` npm packages are **not** preinstalled here either, despite the skill docs saying so — needed `npm install docx` in the scratch working directory before `require('docx')` worked. Separately, system Python on this Mac is 3.9.6 (`/usr/bin/python3`), which is too old for the docx skill's `scripts/office/validate.py` (uses `match` statements, needs 3.10+) and for `soffice.py`'s `ignore_cleanup_errors` tempfile arg (also 3.10+) — the skill's own `soffice.py` wrapper actually fails on this machine's Python for that reason, so call `soffice` directly instead of through that wrapper. **How to apply:** the first time a session needs to create/edit/preview a Word or PowerPoint file, expect to install LibreOffice and `npm install docx`/`pptxgenjs` up front rather than assuming they're there; skip the skill's `validate.py`/`soffice.py` wrapper scripts and call `soffice`/`pdftoppm` directly for visual QA.

See also: [[duo-vert/memory-architecture]], [[feedback/rename-move-verification-checklist]], [[duo-vert/website-build-overview]], [[feedback/gbp-post-content-style]], [[personal/agency-idea]], [[cegep-school-organization]]
