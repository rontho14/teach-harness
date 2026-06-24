# Exam research — building the every-10th-lesson exam

Every 10th new-material lesson, produce a cumulative **exam** in
`exams/NNNN-slug.html`, built from `templates/exam.html`.

## Hard rules

- **Research real exams FIRST.** Before writing anything, look at how real,
  rigorous courses test this subject. **Explicitly seek MIT OpenCourseWare and
  Ivy-League-level (and comparable: Stanford, CMU, Berkeley, Oxford, …) past
  exams, midterms, and finals** for the topic, plus their solution sets. It does
  not have to be Ivy League — but go look at that calibre to set the bar. Use
  `WebSearch`/`WebFetch`; save what you find to `RESOURCES.md`.
- **Never invent from memory.** Each exam question must be grounded in a real
  researched problem (reframed or used directly), and **cited**. If research is
  unavailable, fall back to saved resources, and if those don't cover it, ask the
  learner for a source — do not fabricate.
- **Medium → very hard.** Questions must make the learner *stop and think*,
  roughly **20–30 minutes each**. Use **multi-part** structure (a, b, c, …) that
  builds — e.g. set up, solve, then interpret/extend.
- **Span multiple use cases.** The whole point: a student who only practised one
  framing must still succeed on a *different* one. Pull use cases the lessons did
  NOT directly drill. Test transfer, not memorisation.
- **Cumulative.** Cover the whole course so far, not just the last block. Weight
  toward **shaky** sub-concepts: run `python .claude/scripts/fsrs.py due` and
  `... list` to see low-stability / low-retrievability items and make sure they
  appear.

## Composition

- Typically 3–6 questions (each multi-part) to fill a real exam sitting.
- For each question, record in the exam's `manifest.json` which **sub-concept
  slugs** it tests, so grading feeds FSRS across the breadth of the course.
- **Emit a rubric per part (REQUIRED).** As you write each part, also commit its
  grading standard: for every sub-concept it tests, a one-line descriptor of what
  a 1 / 2 / 3 / 4 answer looks like *for that specific part*. Set this in the
  template's `RUBRICS` object so it lands in `manifest.json`. Do it now, at build
  time, while the question's intent is freshest — grading reads it back instead of
  re-improvising the standard (stops FSRS grade drift).
- Mix question modes: derivation/proof, applied problem on fresh data, "find the
  bug / critique this approach", design/justify, interpret results.
- Calibrate length honestly — state an expected time per question.

## Submission

Configure submission inputs to fit the subject (see `GRADING.md`): typed answers
plus the natural artifact (a `.ipynb`, a photo of worked solutions, etc.). The
Finish button bundles everything into one submission `.zip`.

## After the exam is graded

Grading (see `GRADING.md`) assigns each tested sub-concept a 1–4 and runs
`fsrs.py review`. A weak exam showing on a "consolidated" item is a signal it
wasn't really consolidated — its reschedule will pull it back into rotation.
