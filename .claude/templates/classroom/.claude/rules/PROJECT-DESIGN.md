# Project design — building a Personal Project

A **Personal Project** is the classroom's **Wisdom** artifact (see `PEDAGOGY.md`):
a real-world deliverable that makes the learner *test their skill in the wild*,
tied to a `MISSION.md` success gate. Examples by subject: build and defend your own
training program, run a marathon (gym); ship a real project to GitHub (coding);
construct and track a real portfolio (investing).

A project lives in **`projects/NNNN-slug/`**:
- `brief.html` — built from `templates/project.html` (the goal, deliverable,
  acceptance gates, exercised sub-concepts, check-in cadence, and the check-in form).
- `log.md` — status (`proposed` | `active` | `delivered`) and a dated history of
  check-ins and grades.

## When to start a project

Projects are **not** on a numeric cadence (unlike reviews/exams). Start one when:

- a **mission success gate** becomes reachable — the learner now has enough Skill
  that a real deliverable is in their ZPD (propose it), **or**
- the learner **asks** for one ("start a project", "I want to build X").

Honor an explicit request even if early; otherwise propose at natural milestones
(e.g. the end of a phase in the plan).

## Hard rules

- **Tie it to the mission.** The deliverable must satisfy (or make concrete
  progress toward) a specific `MISSION.md` success gate. State which one in the
  brief's "Why this matters".
- **Make the deliverable concrete and real.** Not "learn about X" — a *thing that
  exists in the world*: a repo, a written-and-defended program, a logged 60-day
  streak, a finished race. The learner produces it outside the classroom.
- **Research the deliverable's real-world shape FIRST.** Look at how this artifact
  is actually built/judged in the wild (real training programs, real open-source
  project conventions, real portfolios) and **cite** it; save finds to
  `RESOURCES.md`. Never invent the standard from memory.
- **Commit acceptance gates up front (REQUIRED).** Write the definition of done as
  a short list of **acceptance gates** while intent is freshest. Each gate is a
  `data-question` block; map it (via `QUESTION_CONCEPTS`) to the sub-concept
  slug(s) it exercises, and emit a `RUBRICS` entry — a one-line 1/2/3/4 descriptor
  per sub-concept for that gate. Grading reads these back (stops FSRS grade drift).
- **Exercise real sub-concepts.** A project applies things already taught. List the
  exercised slugs in `META.sub_concepts`; weight toward **shaky** ones
  (`python .claude/scripts/fsrs.py due`) so real-world use shores them up.
- **Parallel track — never block lessons.** A project runs alongside normal
  lessons over weeks. An active or check-in-due project does **NOT** gate lesson
  N+1, and does **NOT** increment `lessons_taught`. Lessons and the 5/10
  review/exam rhythm continue independently.

## Check-in cadence and lifecycle

- Set a check-in cadence in the brief (e.g. "every ~2 weeks" or "at each gate").
- On each check-in the learner submits the brief's form → a
  `project-NNNN-<date>-submission.zip` with progress notes + evidence (a pasted
  deliverable URL, a photo, a logbook export). Grade it like any artifact
  (`GRADING.md`): grade exercised sub-concepts 1–4 against the committed gate
  rubrics, run `fsrs.py review`, and append the check-in to `log.md`.
- **Status:** set `proposed` when you create the brief, `active` once the learner
  starts, `delivered` when all acceptance gates are met. On delivery, bump
  `course.json.projects_done` (and record a `learning-records/` entry for the
  mission gate satisfied).

## Composition

- Typically 2–5 acceptance gates — enough to define "done" without micromanaging.
- Configure the brief's evidence inputs to fit the subject (deliverable URL, image
  of physical evidence, data/logbook export, etc. — see `GRADING.md`).
- Calibrate honestly: a project should stretch over real time, not be doable in one
  sitting (that's what exercises are for).

## After a check-in / delivery is graded

Grading (`GRADING.md`) assigns each exercised sub-concept a 1–4 and runs
`fsrs.py review`. Strong real-world performance is the **best** evidence of
mastery; weak performance on a "consolidated" item pulls it back into rotation.
Update `log.md`, and on final delivery update `MISSION.md` (gate met) and write a
`learning-records/` entry.
