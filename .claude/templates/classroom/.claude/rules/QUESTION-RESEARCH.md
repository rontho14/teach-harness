# Question research — building a lesson's exercise

Every lesson ships with an **exercise**: 5 questions in `exercises/NNNN-slug.html`
(exercise N pairs with lesson N), built from `templates/exercise.html`.

## Hard rules

- **5 questions.** Each should take the learner roughly **5–10 minutes** and
  require **actual thinking or work** — not one-line recall. Favour: produce
  something, apply a method to new data, debug, derive, choose between options
  and justify, predict-then-check.
- **Research first; never invent.** Find *real* problems on the topic, then
  either reframe them toward the learner's mission or use them as-is. You choose
  per question which is better, but the seed must be a real, citable problem.
- **Cover only what THIS lesson taught.** No material from later lessons. Map
  each question to the lesson's sub-concept slug(s) it exercises — the exercise's
  `manifest.json` records this so grading can feed FSRS per sub-concept.
- **Emit a rubric per question (REQUIRED).** When you write each question, also
  commit its grading standard: for every sub-concept it tests, a one-line
  descriptor of what a 1 / 2 / 3 / 4 answer looks like *for that specific
  question*. Set this in the template's `RUBRICS` object so it lands in
  `manifest.json`. Do it now, while the question's intent is freshest — grading
  reads it back instead of re-improvising the standard (stops FSRS grade drift).
- **Cite every source.** Put a citation on each question and add the source to
  `RESOURCES.md`.

## Where to find real problems (in rough priority)

1. The canonical textbook(s) for the subject — end-of-chapter problem sets.
2. University course pages / problem sets (MIT OCW, Stanford, CMU, Berkeley, …).
3. Official docs' exercises/tutorials (for tools/libraries).
4. High-signal practice sites (e.g. for stats/ML: real datasets; for math:
   competition/qualifying problems; for code: curated katas) and well-moderated
   Q&A (Stack Exchange) — adapt, don't copy low-quality content.

Use `WebSearch`/`WebFetch` to gather these. Save good finds to `RESOURCES.md`
with a one-line "use for" note.

## Reframing toward the mission

A researched problem about topic X can be re-skinned to the learner's domain
(their dataset, their use case) **without changing the underlying skill**. Do
this when it raises motivation/relevance; keep the original citation either way.

## Difficulty and shape

- Order the 5 from a warm-up to a stretch within the lesson's ZPD.
- Prefer questions with a checkable result (so feedback is concrete), but
  open-ended "explain/justify" questions are welcome for understanding.
- Configure the exercise's **submission inputs** to fit the subject (see
  `GRADING.md`): typed answers, a notebook upload (`.ipynb`), a photo of
  handwritten work (`image/*`), etc. The agent decides per subject at build time.

## If research is unavailable or finds nothing suitable

Degrade gracefully — do **not** fabricate:

1. Try sources already saved in `RESOURCES.md` / `reference/`.
2. If those don't cover it, tell the learner and **ask them for a source**
   (a textbook + chapter, a past problem set, a link) before producing the
   exercise.
