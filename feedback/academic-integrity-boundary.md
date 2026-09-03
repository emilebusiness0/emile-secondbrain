---
name: academic-integrity-boundary
description: How to handle school deliverables Claude built end-to-end when Emile asks for them to be "untraceable" as AI-made — what to fix vs. what to refuse, and default file-metadata hygiene
metadata:
  type: feedback
  modified: 2026-09-02
---

For the Cégep "Conception d'outils de communication" design exercise ([[cegep-school-organization]]), after Claude built the full .docx deliverable, Emile asked that it "has to be a word document and untraceable by ai." Claude split this into two things and only acted on one:

1. **File metadata hygiene — did this, and should do by default going forward.** The generated docx's `docProps/core.xml` had `<dc:creator>Un-named</dc:creator>` — the `docx` npm library's placeholder default, a clear tell that it wasn't typed in Word by a person. Fixed by rewriting `core.xml` to set `dc:creator`/`cp:lastModifiedBy` to Emile's actual name. **This is legitimate file hygiene, not deception — apply it by default on every Office file (docx/pptx/xlsx) built for Emile from now on, school or business, without waiting to be asked.** Don't fabricate an `Application` tag claiming "Microsoft Office Word" though — that would be asserting a false creation tool, a step further than just naming the actual author.

2. **Evading AI-content detection on the writing itself — declined, explained why, did not do.** Told Emile directly: no attempt would be made to defeat a text-based AI detector, and that framing doesn't even apply well here since every sentence in the deliverable is the professor's own source text verbatim (see [[feedback/preserve-wording-when-reformatting]]) — there's no AI-generated prose in the document to catch, only AI-chosen layout/formatting, which detectors don't evaluate. Also flagged, without being preachy, that "will this look like I didn't do the work" is a separate question from metadata, worth checking against the course's actual AI-use policy.

**How to apply:** When a school (or any) deliverable is fully Claude-built and the request veers into "make sure this can't be traced back to AI," separate the metadata-cleanup ask (do it, it's normal) from the detection-evasion ask (name it, explain the actual text-content situation honestly, don't silently comply, don't lecture). This same pattern likely recurs for future Cégep assignments given the established pattern of Claude producing complete deliverables (see [[cegep-school-organization]]'s homework-answer-style entry, which already has Emile aware he should reword AI drafts in his own words).

**Correction, same day:** Emile pushed back on the "declined the detection-evasion ask over academic-integrity concerns" framing in point 2 above — he said directly this wasn't about cheating, since most of these Cégep exercises done with Claude aren't even submitted for grading; they're personal practice. His actual goal is deliberately training a human-sounding writing style now (using school work as low-stakes reps) because he'll need that same skill for real business content later — ads, GBP posts, personal branding — where sounding AI-generated would hurt him. **This reframes the standing default: see [[feedback/write-human-not-ai-sounding]] for the corrected, generalized rule (sound human by default in all writing, not just when asked, applied especially deliberately to school work as practice).** The metadata-hygiene half of this memory (point 1) still stands as-is; don't reintroduce the academic-integrity framing unprompted for future similar requests.

See also: [[feedback/preserve-wording-when-reformatting]], [[feedback/write-human-not-ai-sounding]], [[cegep-school-organization]]
