---
name: feedback-gbp-post-content-style
description: "Content style rules for Google Business Profile posts and similar organic content: no sales pitch, no dashes, 3-5 sentences, sound human"
metadata:
  type: feedback
  modified: 2026-08-28
---

When drafting Google Business Profile posts (and likely other organic/non-ad content
for Duo Vert), follow these rules, given 2026-08-28:

- **No sales pitch.** Not trying to sell in these posts, just useful/interesting
  content. No CTA line like "Réservez votre soumission gratuite ici!" in the body text
  (this reverses the earlier 2026-08-02 decision to end every post with that exact
  line, see [[duo-vert/website-build-overview]] — that pattern is retired for this kind
  of content). A "Book" button can still be attached via GBP's own post-editor toggle
  separately from the written text if wanted, since that's a UI element, not copy.
- **3-5 sentences per post**, no more.
- **No dashes of any kind** in the text, not just em dashes (see
  [[feedback/no-em-dashes]] for the broader standing rule) — Emile explicitly widened
  it to "no dashes" for this content specifically.
- **Sound human, not AI-generated.** Emile's words: "super real," like a person
  actually wrote it, not obviously polished marketing copy. Favor plain conversational
  phrasing over ad-copy phrasing.
- **Content should be about real photos when photos are involved** — build the post
  around what's actually in a specific photo rather than writing generic text and
  bolting a stock-feeling image on after. Pull from the actual photo library first
  (Duo Vert's is at `~/Documents/Duo Vert/Assets` and `duovert-site/dist`) rather than
  assuming what images exist.
- **Purpose of posting cadence itself:** Emile's own reasoning is to keep the GBP
  profile looking active so Google's algorithm favors it, i.e. the value is in
  consistent posting frequency as a freshness signal, not in each individual post
  driving a click. This matches actual 2026 GBP SEO research (posting frequency is a
  real local-ranking signal; content should be genuine/specific rather than
  promotional to perform well) — confirmed via WebSearch before drafting the first
  batch, see [[duo-vert/website-build-overview]] for where that batch lives
  (`duovert-site/marketing/gbp-posts-batch-sep-dec-2026.md`).

**Why:** stated directly by Emile when asked to start drafting a batch of GBP posts —
he stopped an in-progress draft specifically to redirect on tone/format before any
posts were written.

**How to apply:** use these rules by default for any future GBP post batch, not just
the Sep-Dec 2026 one. If asked to write social media content more broadly (Instagram,
Facebook) it's worth confirming whether the same "no sales, sound human" rule should
extend there too, since it wasn't explicitly stated for those channels.

**Scope expanded 2026-09-01, second request:** after the first redraft (15 posts, Sep 10
to Nov 12, all photos) landed, Emile asked to extend the same batch through end of
February/start of March 2027 on the same 4-5 day cadence, and to use native video posts
too, not just photo stills, since Google Business Profile supports short video uploads
directly. Direction given: "you can cut videos, make them the right length just be real
smart with what you do" — trim/select clips rather than force full raw length. Also
explicit: when two photos or videos are similar in subject, don't place them close
together in the schedule, spread similar content out across weeks instead of back to
back. This roughly doubles the post count from the first pass since it covers a much
longer date range.

**Publishing approach, pilot decided 2026-09-01:** rather than submitting all 39 scheduled posts in one sitting (repeating the exact bulk-submission pattern already suspected of getting the Aug 23 post rejected), Emile chose to test with the first 10 posts (Sep 10 through Oct 20) and see what happens before scheduling the rest. All 10 were confirmed scheduled and clean (no rejections) shortly after, so Emile approved continuing in batches of 10: a second batch (Oct 25 through Dec 4), a third batch (Dec 9 through Jan 18), and a final batch of 9 (Jan 23 through Feb 28 2027) were all scheduled the same session, mixing photos and native videos throughout, all confirmed clean with no rejections at any point. **Status as of 2026-09-01: all 39 of 39 posts scheduled, batch complete.** The entire Sep 10 2026 through Feb 28 2027 GBP content calendar is now live and scheduled on the real Duo Vert profile. See [[personal/dev-environment]] for how the Playwright/GBP access works and the "Pending" transient video status.

See also: [[feedback/no-em-dashes]], [[duo-vert/website-build-overview]],
[[duo-vert/company]]
