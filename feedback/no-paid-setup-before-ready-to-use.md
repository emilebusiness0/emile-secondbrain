---
name: no-paid-setup-before-ready-to-use
description: "Emile's spending rule: don't activate or pay for any service/tier until it will actually be used live, even mid-build"
metadata:
  type: feedback
---

**Stated 2026-09-01**, in the context of CRM work and the AI phone
receptionist idea. Emile drew a clear line: build/wire up the free parts
of a system now (e.g. Google Cloud OAuth app for Gmail - no cost), but do
not fund or activate anything that costs money (Sent account for SMS,
paid Turso hosting tier, phone/voice API usage) until it's actually ready
to go live and be used, even if that means a feature is "built but not
turned on" for a while.

**Why:** don't pay for capacity or services sitting idle. Fits his broader
self-awareness about avoiding scope creep/spend creep while still chasing
multiple ideas at once (see [[personal/motivation-and-mission]],
[[feedback/proactive-opinions-and-next-steps]]).

**How to apply:** when a build needs a paid step (hosting tier, SMS/voice
credits, a paid API tier, a subscription), check whether the feature will
actually be exercised soon. If not, build the free/structural parts and
explicitly flag the paid step as a deferred action item rather than doing
it automatically - don't assume "finishing the feature" implies "turning
on the paid part."

See also: [[duo-vert/custom-crm-prototype]], [[personal/agency-idea]],
[[feedback/proactive-opinions-and-next-steps]].
