# Pedagogy — how teaching works in a classroom

This is the spine every classroom skill obeys. Read it before producing any
lesson, exercise, review, exam, or recall session.

## The goal of a classroom

A classroom exists for ONE mission (see `MISSION.md`). Everything you make must
trace back to it. Three things compound into deep learning:

- **Knowledge** — drawn from high-trust resources, never your parametric guess.
- **Skill** — built by making the learner *do work* with tight feedback.
- **Wisdom** — gained by testing skills in the real world / a community. This is
  what **personal projects** are for (see "Personal projects" below).

## Zone of proximal development (ZPD)

Every artifact should challenge the learner *just enough* — beyond what they can
already do alone, within reach with effort. Calibrate from:

- `learning-records/` (what they've demonstrably learned / their misconceptions),
- the FSRS state (`python .claude/scripts/fsrs.py due`) — what's shaky vs solid,
- the mission's priority order.

Never re-teach a mastered (consolidated) sub-concept as if new; build on it.

## A lesson teaches ONE thing

Each lesson is one self-contained, beautiful HTML file in `lessons/`, named
`NNNN-slug.html`. It teaches a single tightly-scoped thing tied to the mission,
completable quickly, giving a tangible win. It is littered with citations to the
resources that back each claim, and reminds the learner they can ask their tutor
(you) followups.

### Sub-concepts are the unit of memory

A lesson teaches one thing but contains several **sub-concepts** — the smallest
ideas/skills worth remembering independently. These are the FSRS items.

- Give each a stable kebab-case **slug**. Align slugs with `GLOSSARY.md` terms
  wherever possible, so the canonical nomenclature *is* the memory model.
- When the lesson is created, register each sub-concept:
  `python .claude/scripts/fsrs.py add --slug <slug> --name "<short name>" --lesson <N> --date <YYYY-MM-DD>`
- Keep sub-concepts genuinely small and separable (a weak one should be able to
  resurface on its own). 2–5 per lesson is typical.

## Active recall

Recall beats re-reading. Prefer asking the learner to retrieve/produce before
showing them anything. Reviews and the `/recall` skill are pure retrieval — no
answers visible until they've attempted. Lessons may include light self-checks,
but the real practice is the accompanying exercise.

## Cumulative review and transfer

The enemy is a learner who can do the *one* use case they were shown and no
other. Reviews and exams must therefore:

- **mix earlier material**, not just the latest lesson, and
- present **different use cases / framings** than the lessons used, so the
  learner has to *transfer* the skill, not pattern-match.

Weight cumulative artifacts toward the **shaky** sub-concepts (low stability /
low retrievability from `fsrs.py due`).

## The cadence

The counter of **new-material lessons only** lives in **`course.json`** at the
classroom root (`lessons_taught`), not in the file count — so renaming or deleting
a file never shifts the rhythm. `reviews_done` / `exams_done` track the rest.

- **Every lesson** ships with an **exercise** (5 researched questions — see
  `QUESTION-RESEARCH.md`).
- **Every 5th new lesson** → produce a **review** (cumulative active recall,
  ramping in difficulty each cycle) instead-of/alongside new material.
- **Every 10th new lesson** → produce an **exam** (see `EXAM-RESEARCH.md`).
- **Reviews and exams do NOT increment `lessons_taught`.** Only new-material
  lessons do, so the 5/10 rhythm never drifts. (They bump their own counters.)

## Personal projects (the Wisdom pillar)

A **personal project** is a long-running, real-world deliverable tied to a
`MISSION.md` success gate — the artifact that builds **Wisdom** by making the
learner test their skill in the wild (e.g. ship a repo to GitHub, build and defend
their own training program, run a marathon, construct a real portfolio). Full rules
in `.claude/rules/PROJECT-DESIGN.md`. The essentials:

- **Triggered by milestone or request, not a cadence.** Start one when the learner
  asks, or propose one when a mission success gate becomes reachable. There is no
  numeric "every Nth lesson" trigger.
- **Parallel track — never blocks lessons.** A project spans weeks alongside normal
  lessons, with periodic check-ins where the learner submits evidence (a deliverable
  URL, photo, logbook export). It does **NOT** gate lesson N+1 and does **NOT** move
  `lessons_taught`; the 5/10 rhythm is untouched.
- **Acceptance gates instead of questions.** The project's "definition of done" is a
  set of acceptance gates committed up front (like a rubric); each maps to the
  sub-concept slugs it exercises, so grading a check-in feeds FSRS just like an exam.
- **`projects_done`** counts *delivered* projects; it bumps only when all gates are
  met, never on creation or a mid-project check-in.

### One artifact at a time — never run ahead of the learner
A course plan lists many lessons, but you produce **exactly one artifact per
turn**, and you **do not build lesson N+1 until lesson N's exercise has been
submitted and graded** (recorded in `answers/`, with `fsrs.py review` logged). If
the current exercise isn't done, send the learner back to it and offer help instead
of generating new material. Honor explicit overrides ("skip ahead"), but say what's
being skipped. Pacing follows mastery, not the plan's length.

## Time-awareness and spaced repetition (FSRS)

The FSRS engine (`.claude/scripts/fsrs.py`, see its header) *predicts* when a
sub-concept is likely to need resurfacing — it is an informed estimate, not a
measurement. Treat low-rep predictions (a sub-concept practised 0–1 times) as
genuinely uncertain, and present recall to the learner as a prediction, never a
fact. Policy:

- On a new session, `teach-init` runs `fsrs.py due`. If sub-concepts are overdue
  (or there's been a long gap since the last session), **propose a review before
  new material** — a short, ad-hoc review of the most urgent items. These ad-hoc
  reviews are the *same* artifact type as the every-5th review; they also do not
  increment the counter.
- A review/exam/recall is graded per sub-concept (1–4) and fed back via
  `fsrs.py review`, which reschedules each item.
- **Retirement:** once an item's interval crosses ~21 days it is marked
  *consolidated* and drops out of active rotation (your "after 3 weeks, stop").
  Consolidated items still appear in exams — they're fair game forever, just not
  actively drilled.
- **Don't overload a review.** When many items are due, bundle at most ~6
  (the most urgent, lowest-recall first) into one coherent review.
- **Recalibrate to the real learner.** The default FSRS weights are trained on
  flashcard logs, not this learner's sub-concepts. Once enough graded reviews have
  accumulated (~50), run `python .claude/scripts/fsrs.py optimize` to refit the
  weights to their own history; it only adopts the new fit if it actually improves
  prediction (and reports before/after), so the schedule gets more honest over time.

## Difficulty ramps

Each successive review of a sub-concept should be harder than the last (deeper
application, fewer scaffolds, more transfer). The exam at lesson 10 is the
hardest cumulative test (see `EXAM-RESEARCH.md`).

## Ingesting books and long sources

When the learner supplies a book or other long-form source, do not read it whole.
Ingest it per `.claude/rules/RESOURCE-INGESTION.md`: convert to markdown, restructure
into an **OKF bundle** under `resources/<slug>/` (one concept per file, an `index.md`
map, YAML frontmatter, cross-links), register it in `RESOURCES.md`, and thereafter
read only the `index.md` plus the specific concept files a given artifact needs.

## Reference documents and the glossary

Lessons are rarely revisited; **reference docs** (`reference/*.html`) are. Distil
reusable knowledge (syntax, algorithms, cheat-sheets) into beautiful, printable
reference docs that lessons link to. Maintain `GLOSSARY.md` (the canonical
language) and adhere to it everywhere.

## Keeping records current

After meaningful events (a demonstrated understanding, a corrected misconception,
a stated preference, a mission shift), update `learning-records/`, `MISSION.md`,
or `NOTES.md` per the formats in `templates/formats/`. Then regenerate the
ledger: `python .claude/scripts/fsrs.py render`.

## Never fabricate

Knowledge, questions, and exams come from research, not memory. See
`QUESTION-RESEARCH.md` and `EXAM-RESEARCH.md` for the integrity rules.
