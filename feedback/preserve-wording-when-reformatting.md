---
name: preserve-wording-when-reformatting
description: When a task is "reformat/redesign this text" (not "rewrite" or "improve the writing"), keep every original word intact — restructure only, never paraphrase
metadata:
  type: feedback
  modified: 2026-09-02
---

When asked to reformat or redesign an existing text (e.g. apply graphic design principles to a document, turn plain text into a flyer/layout), do not paraphrase, drop clauses, or change verb tenses while restructuring — every original word must still appear, only the structure/layout may change (paragraph → bullets/table, adding headings, bolding for emphasis).

**Why:** Built a Cégep assignment deliverable ([[cegep-school-organization]]) that asked to apply the 4 graphic design principles (contraste, répétition, alignement, proximité) to a raw announcement text. While restructuring prose into bullets/a schedule table, several real edits crept in without noticing: a clause was dropped ("L'événement se déroulera"), one sentence was rewritten wholesale, several verbs were shifted tense (sera→est, pourra→peut, nécessiteront→nécessitent), and one sentence was replaced by an invented heading. Emile caught this by directly asking "is every word from the initial document still there — can't remove a single word?" A visual read-through of the rendered PDF had NOT caught any of this, because the changes read naturally and looked like normal copy. Only an actual word-by-word programmatic diff against the source text surfaced the exact list of omissions/rewrites.

**How to apply:** For any "reformat this document" / "redesign this text" / "apply design principles to this text" task:
1. Treat wording as read-only — the deliverable's structure changes, its sentences don't.
2. Headings/labels are the one exception: reusing or all-caps'ing a phrase that's already in the source text as a heading is fine (nothing is lost, it's just also displayed as a title). Inventing brand-new heading text that *replaces* an original sentence is not — keep the original sentence too.
3. Splitting a run-on paragraph into a table (e.g. a schedule) is fine as long as the full original sentence still appears somewhere (e.g. in an "Activité" column) — don't let only fragments (just the time, just the label) survive the split.
4. Before declaring the deliverable done, run an actual word-by-word diff/count of the source text against the extracted text of the final file (not just a visual/eyeball read of the rendered PDF) — this is the only check that reliably catches a dropped clause or a tense change, since a plausible rewrite reads fine on a visual pass.

See also: [[feedback/no-inventing-when-citing-sources]] (same family of "don't quietly alter someone else's content"), [[cegep-school-organization]]
