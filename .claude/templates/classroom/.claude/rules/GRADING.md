# Grading — turning a submission into feedback + FSRS updates

The learner does an exercise/review/exam, clicks **Finish**, and a single
submission `.zip` downloads. Then they ask you to review it (e.g. "review my
answers" — they do NOT need to name the artifact; the zip says what it is).

## Finding the submission

- Look ONLY at the **3 most-recently-modified files** in the learner's Downloads
  folder: `C:\Users\ronth\Downloads`. (Keeps you out of the rest of their disk.)
- Pick the **newest `.zip`** among those three. Announce which file you're
  grading before opening it, so the learner can correct you.
- Unzip it (PowerShell): `Expand-Archive -Path <zip> -DestinationPath <tmp> -Force`.

## Reading the submission

Inside the zip:
- `manifest.json` — artifact type (exercise/review/exam/**project**), number,
  title, date, the list of **sub-concept slugs** each question (or project
  **gate**) covers, and each one's **`rubric`**: the pre-committed 1–4 descriptors
  (per sub-concept) written at build time, when the intent was freshest. This tells
  you which artifact it is and what to grade against. A **project** manifest also
  carries `deliverable`, `gates`, and `deliverable_url`.
- `answers.md` — the learner's typed answers (for a project: progress notes per
  acceptance gate).
- attached files — a `.ipynb` (read it cell by cell), image(s) of handwritten
  work (read them visually with the Read tool), a project's evidence (a logbook
  export, a photo, a repo linked via `deliverable_url`), etc.

Open the matching artifact in `exercises/`, `reviews/`, `exams/`, or
`projects/NNNN-slug/brief.html` and the sources it cited so you grade against the
real questions/gates and rubric.

**Project check-ins** are graded the same way, against the brief's committed
**acceptance gates**: judge the real evidence (visit the `deliverable_url`, read
the logbook/photo), grade each exercised sub-concept 1–4 against its gate rubric,
then append the check-in to the project's `log.md`. Mark the project `delivered`
and bump `course.json.projects_done` only when **all** gates are met.

## Grading and feedback

- Grade each question on correctness AND reasoning, grounded in the cited
  sources — not your unaided opinion. Reproduce/quote a source when it settles a
  point.
- Give **specific, kind, useful** feedback: what was right, what was wrong and
  *why*, and the smallest fix. Praise genuine insight; name misconceptions
  plainly.

## Per-sub-concept FSRS grades

For each sub-concept the artifact covered, assign a 1–4 from the learner's
performance on the questions touching it. **Grade against the pre-committed
rubric**, not your unaided judgement: for each question, find its `rubric` entry
in `manifest.json` for the sub-concept in question, match the learner's answer to
the closest 1–4 descriptor, and **quote that matched line in your feedback** so
the standard is visible and the grade is reproducible. This is the primary anchor
— it stops the schedule drifting as the grading standard shifts session to
session. When a question touches several sub-concepts, grade each against its own
rubric line.

If (and only if) a question has no `rubric` in the manifest (e.g. an older
artifact), fall back to this generic mapping:

| Grade | Meaning |
|---|---|
| 1 (Again) | Couldn't do it / fundamental error |
| 2 (Hard)  | Got there with hints, or with real gaps |
| 3 (Good)  | Solid, only minor slips |
| 4 (Easy)  | Effortless, fluent, could teach it |

Then record each: `python .claude/scripts/fsrs.py review --slug <slug> --grade <1-4>`
(add `--date YYYY-MM-DD` if grading for a past day).

## Record-keeping

1. Copy the unzipped submission into `answers/<artifact>-<date>/` as the
   permanent record.
2. Write a `learning-records/NNNN-*.md` entry for anything decision-grade: a
   demonstrated mastery, a corrected misconception, a stated preference, a
   mission shift (formats in `templates/formats/`).
3. Regenerate the ledger: `python .claude/scripts/fsrs.py render`.
4. Tell the learner where they stand and what's next (the ZPD next step, or which
   shaky sub-concepts will resurface soon).
