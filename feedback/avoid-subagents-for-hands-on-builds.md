---
name: avoid-subagents-for-hands-on-builds
description: For build/implementation work Emile is actively directing (like the CRM prototype), do the work directly instead of spawning Agent/Plan/Explore subagents — he finds them token-expensive and not useful for this kind of close, iterative work
metadata:
  type: feedback
  modified: 2026-08-28
---

Stated explicitly 2026-08-28, mid-way through CRM prototype planning, after I'd
dispatched a Plan subagent to design the architecture: "stop running agents. They use
way too many tokens and I don't think it's really useful for the moment."

**Why:** for work he's actively, closely directing — a build he wants to shape
feature-by-feature, iterating in conversation — a subagent's output is a large, opaque
chunk of work he didn't get to steer as it happened, and it costs real tokens to
produce. This is different from research/exploration tasks where a subagent genuinely
saves context and time by doing legwork Emile doesn't need to see. He said "for the
moment," so treat this as scoped to hands-on build work he's directly steering (like the
[[project/duovert-custom-crm-prototype]] build), not necessarily a permanent ban on
every subagent use — but default to caution before spawning one on his active projects.

**How to apply:** for build/implementation tasks Emile is personally directing in real
time, do the work directly (Read/Write/Edit/Bash, WebSearch myself if needed) instead of
delegating to Agent/Plan/Explore/Workflow. Reserve subagents for genuinely
parallelizable or large-context research tasks where he isn't trying to steer the
output step by step — and if in doubt, ask first rather than spawning one on a project
like this.

**Reconfirmed 2026-08-29:** during plan-mode work on the CRM (which structurally
launches Explore subagents for phase 1), Emile flagged the cost directly: "it says
your agent uses like 170k tokens... my usage hasn't really got up a ton but try to
keep the tokens moderate." One Explore agent alone burned ~170k tokens on this
project (two others ran ~60-75k each in the same batch). Lesson: even when a
mode/workflow nudges toward spawning Explore agents (e.g. plan mode's phase 1),
prefer reading the specific files directly myself when the codebase area is
already well-understood from prior rounds — reserve Explore/Plan agents for
genuinely unfamiliar territory, and default to 1 targeted agent (or zero) rather
than 3 parallel ones, on this project specifically.

See also: [[feedback/ask-detailed-specs-before-building]],
[[project/duovert-custom-crm-prototype]]
