---
name: new-classroom
description: Scaffold a new classroom (a subject subfolder) inside the organizer from the bundled template, seed its mission/notes/resources, initialize the FSRS schedule, and generate the first lesson + exercise. Use after /teach intake (and optional /grill-me) when the user is ready to start learning a subject.
argument-hint: "<subject> (a short name for the folder, e.g. linear-algebra)"
---

You are turning a refined plan into a working **classroom**. Run this from the
**organizer** root.

## 1. Resolve the subject
- Take the subject from the argument, or infer it from the most recent
  `.claude/plans/<subject>.md`. Confirm a short kebab-case folder name `<subject>`.
- If no plan exists yet, run `/teach` intake first (don't scaffold ungrounded).

## 2. Copy the bundle (PowerShell)
```
$dst = "<subject>"
New-Item -ItemType Directory -Force -Path $dst | Out-Null
Copy-Item -Recurse -Force ".claude\templates\classroom\.claude" "$dst\.claude"
New-Item -ItemType Directory -Force -Path "$dst\learning-records","$dst\lessons","$dst\exercises","$dst\reviews","$dst\exams","$dst\projects","$dst\answers","$dst\reference","$dst\resources" | Out-Null
```
The copied `.claude` is a complete classroom (skills teach/teach-init/recall,
scripts/fsrs.py, rules/, templates/). It deliberately does NOT contain
`new-classroom` or the meta-bundle, so there's no recursion.

## 3. Seed the classroom files
From the plan and the intake conversation, write into `<subject>/`:
- **`MISSION.md`** — per `templates/formats/MISSION-FORMAT.md`.
- **`NOTES.md`** — preferences, **including the chosen teaching language** and any
  always/never rules.
- **`RESOURCES.md`** — the researched shortlist, per `RESOURCES-FORMAT.md`.
- **`GLOSSARY.md`** — an empty stub with the topic heading (fill as terms are owned).

## 4. Initialize the schedule and course counters
```
cd <subject>
python .claude/scripts/fsrs.py init
```
Also write **`course.json`** at the classroom root — the source of truth for the
milestone counters (so the 5/10 review/exam rhythm never depends on file counts):
```
{ "lessons_taught": 0, "reviews_done": 0, "exams_done": 0, "projects_done": 0, "last_session_date": "<YYYY-MM-DD>" }
```
(You'll set `lessons_taught` to 1 in step 5 after lesson 1 is created.)

## 5. Generate lesson 1 (+ exercise 1)
Now act as the classroom tutor (follow `.claude/rules/PEDAGOGY.md` and
`.claude/rules/QUESTION-RESEARCH.md`):
- Create `lessons/0001-<slug>.html` from `.claude/templates/lesson.html` — one
  tightly-scoped first thing in the learner's ZPD, tied to the mission, every
  claim cited.
- Register its sub-concepts: `python .claude/scripts/fsrs.py add --slug <slug> --name "<name>" --lesson 1` (once each).
- Create `exercises/0001-<slug>.html` from `.claude/templates/exercise.html` — 5
  **researched** questions, `META`/`QUESTION_CONCEPTS`/`RUBRICS` set (the
  per-question 1–4 grading standard, committed now), submission inputs configured
  for the subject.
- Set `lessons_taught` to **1** and `last_session_date` to today in `course.json`.
- Refresh the ledger: `python .claude/scripts/fsrs.py render`.

## 6. Hand off
Tell the user, with the real paths filled in:
> Classroom ready at `./<subject>/`. Open your first lesson:
> `start chrome "<absolute path to lessons/0001-...html>"`
> Then **`cd <subject>`** and start a new chat there — the session hook will load
> your tutor context automatically. Say "next" to continue, or "review my answers"
> after you finish the exercise.
