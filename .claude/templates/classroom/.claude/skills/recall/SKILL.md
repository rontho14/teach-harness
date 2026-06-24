---
name: recall
description: Run a live Socratic active-recall session in a classroom. The tutor interrogates the learner from memory on already-taught sub-concepts, one escalating question at a time, then converts their performance into FSRS grades so the schedule updates. Use when the user wants to be quizzed, drilled, or to test what they remember ("quiz me", "recall", "test me").
argument-hint: "(optional) topic or sub-concept to focus on"
---

A fast, conversational active-recall drill — pure retrieval, no peeking. This is
one of the highest-value learning activities; it both strengthens memory and
measures it for the FSRS schedule.

## Setup

1. Make sure context is loaded (run `teach-init` if it hasn't this session).
2. Choose what to drill:
   - If the user named a topic, focus there.
   - Otherwise run `python .claude/scripts/fsrs.py due` and drill the most urgent
     (lowest-recall / overdue) sub-concepts first; mix in a few solid ones so it
     isn't all painful.
3. Aim for **~8–10 questions** total, covering a handful of sub-concepts.

## How to question (Socratic, escalating)

- **One question at a time.** Wait for the answer before the next.
- Start with a plain retrieval prompt, then **escalate**: ask *why*, ask them to
  apply it to a **new** case, ask them to find the flaw, ask them to teach it back.
- **No notes, no peeking** — this is retrieval. Don't reveal the answer until they
  attempt.
- When they answer: confirm what's right, correct what's wrong **with the reason**
  (grounded in `RESOURCES.md`/`reference/`, not your unaided opinion), then move on.
- Keep it brisk and encouraging. Follow `NOTES.md` (language, tone).

## Grade and update the schedule

Track, per sub-concept, how the learner did across the questions touching it, and
map to the FSRS 1–4 scale (see `.claude/rules/GRADING.md`):

| 1 Again | couldn't retrieve / fundamental error |
| 2 Hard  | retrieved with hints or real gaps |
| 3 Good  | solid, minor slips |
| 4 Easy  | fluent, could teach it |

Record each at the end of the session:

```
python .claude/scripts/fsrs.py review --slug <slug> --grade <1-4>
```

Then `python .claude/scripts/fsrs.py render` to refresh `PROGRESS.md`.

## Close out

- Give a short, honest debrief: what's solid, what's shaky, what will resurface.
- Write a `learning-records/NNNN-*.md` entry for anything decision-grade (a
  mastered concept, a stubborn misconception) per the format in
  `.claude/templates/formats/`.
- If several items graded 1–2, suggest the `teach` skill build a full review next.
