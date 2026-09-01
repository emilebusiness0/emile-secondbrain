---
name: cegep-school-organization
description: "Emile's Cégep course list (Automne 2026) and the file organization system set up for schoolwork"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-26T23:25:06.813Z
  originSessionId: a7c3de25-144d-4948-b982-8fef84e44b87
---

Emile started this Cégep semester (Automne 2026) and asked (2026-08-26) for a system to keep schoolwork organized on his new Mac — specifically so Downloads doesn't fill up with homework instructions and assignment files that never get sorted.

**Courses (Automne 2026, from `Documents/Cegep/Cegep Automne 2026.png`):**
- Gestion numérique des organisations (410-152-HU, Yacoub)
- Écriture et littérature (601-101-MQ, Taggart)
- Philosophie et rationalité (340-101-MQ, Bastien)
- Conception d'outils de communication (410-151-HU, Nicole)
- Initiation à la gestion (410-153-HU, Lévesque)
- Contexte légal et exercice de la profession (310-801-HU, Vincent)

**System set up 2026-08-26:** created `~/Documents/Cegep/<Course Name>/` — one folder per course, a top-level sibling of `Documents/Duo Vert/` and `Documents/emile-secondbrain/` (see [[personal/mac-file-organization]]).

**Correction 2026-08-26 — content topic does not reliably indicate the course.** First pass, guessed course assignment from each file's text content. Emile corrected two of three guesses:
- "Devoir forme juridique.docx" (content: legal business-structure cases) — guessed Contexte légal et exercice de la profession, actually belongs to **Initiation à la gestion**.
- "Exercices_IA_COTER.docx" — content read like real Gestion numérique coursework (COTE-R prompting method), but Emile said this is actually just the auto-downloaded instructions file his school platform forces you to download to view (not an assignment to keep) — he had it trashed.
- "devoir IA gestion numérique.docx" — confirmed correct, belongs to Gestion numérique des organisations.

**How to apply going forward:**
- Don't infer the course from file content/topic alone — profs assign cross-topic homework (e.g. legal case studies inside a management course). Ask Emile which course a file belongs to when it's not obvious from the filename or he hasn't said, rather than guessing from content.
- When something downloads that's just the school platform's forced-download instructions/viewer copy (not a deliverable), it can go straight to Trash rather than into a course folder — ask if unsure whether a given file is "instructions to view" vs. "actual homework to keep."
- When Emile mentions homework/schoolwork, or a new file shows up in Downloads that matches a course he's named, sort it into the matching `Documents/Cegep/<Course>/` folder.
- Don't move a file that's currently open in an app (check for a matching `~$<name>` Word lock file in the same folder first) — moving/trashing while open is safe on macOS (the app keeps writing to the same inode) but tell Emile Save As straight into the course folder is the cleaner habit going forward instead of saving to Downloads then moving.
- If a new semester starts and course names repeat/change, ask before restructuring rather than assuming the old folder set still applies.

**Business file convention set 2026-08-26:** client contracts go in `Documents/Duo Vert/Admin/Contrats/` (new subfolder of `Admin/`) — not loose in `Admin/` root. Extend this pattern for any future contract-type document. (Note: the same day, all of `Admin/` moved under a new top-level `Documents/Duo Vert/` folder — see [[personal/mac-file-organization]]'s 2026-08-26 restructure entry — and the Cégep schedule image that used to live at `Admin/Cegep/` moved out to `Documents/Cegep/` directly, since it's school content, not business.)

**Homework-answer style (2026-08-27, Initiation à la gestion reading questions):** when drafting answers to homework questions for Emile to reword in his own words, use simple, everyday vocabulary (accessible to a teenager), not academic/formal language — he asked for this explicitly. Write in first person ("je" for individual answers, "nous"/"on" for answers about a team activity) rather than an impersonal/generic tone — he asked for this too, same session. Combine with [[feedback/no-inventing-when-citing-sources]]: every claim in a homework answer must be traceable to the actual source text (article/posting), simplified in wording but not embellished in content.

See also: [[personal/mac-file-organization]], [[personal/about-emile]]
