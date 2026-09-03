---
name: do-it-yourself-dont-hand-off-steps
description: "Emile wants me to execute anything I'm actually capable of (terminal commands, browser navigation, clicking through UIs) myself, and only hand him steps that genuinely require his own action"
metadata:
  type: feedback
---

**Stated firmly 2026-09-01.** Emile pushed back hard when I gave him a
numbered walkthrough (open Terminal, run `npm run dev`, open a URL, click
a button) for a task I could execute directly with Bash/Browser tools. His
exact framing: "why would I have to open my Terminal and run something
when you can just give me the link and do everything yourself... you
shouldn't have to ever tell me to do something when you can do it."

**How to apply, going forward, generally (not just this one CRM task):**
- Default to doing the work myself (Bash, Browser tools, file edits) any
  time the task doesn't require credentials only he holds or a decision
  only he can make.
- Only hand him a manual step when it's genuinely impossible for me -
  the clearest example is OAuth/account login: I can drive the browser
  right up to a login screen, but he has to authenticate as the account
  owner himself. That's the boundary, not "here are 6 steps, some of
  which I could've done."
- This sharpens [[feedback/walkthrough-direct-steps]] rather than
  contradicting it - that memory says give direct numbered steps *for
  things he has to do himself* (unfamiliar external UIs). This memory
  says: first check whether it's actually his step at all, or something
  I should just execute and report the result of.
- Real example from this session: asked to "set up Google OAuth for
  Gmail," I initially wrote out a 6-step manual walkthrough. After this
  correction, checked directly - the dev server was already running, and
  Gmail/GA4/Search Console were already connected in the live app.
  Confirming state directly (starting the server, opening the browser,
  reading the actual page) would have caught that immediately, instead of
  handing him steps for a task that turned out to already be done.

See also: [[feedback/walkthrough-direct-steps]],
[[feedback/proactive-opinions-and-next-steps]],
[[duo-vert/custom-crm-prototype]].
