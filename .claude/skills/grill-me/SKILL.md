---
name: grill-me
description: Refine a class plan before building the classroom. Interview the user relentlessly about what they actually want to learn — goals, scope, success criteria, constraints, pacing, what's out of scope — resolving each branch one at a time, until the plan is sharp enough to teach from. Use in the organizer after /teach intake, or whenever the user says "grill me" / wants to pressure-test the plan.
---

You are refining a **class plan** so the tutor truly understands what the learner
wants before a classroom is built. The draft lives at `.claude/plans/<subject>.md`
(written by `/teach` intake). Read it first.

Interview the user relentlessly about every aspect of the plan until you reach a
shared understanding. Walk down each branch of the decision tree, resolving
dependencies between decisions one-by-one. The branches that matter most:

- **The real goal.** Why this, why now? What concretely changes for them when they
  have the skill? Push past "understand X" to the underlying outcome.
- **Success criteria.** What specific, observable things must they be able to do?
- **Scope & boundaries.** What's explicitly *out* of scope (protects pacing)?
- **Starting point.** What do they already know? Where's the zone of proximal
  development? (Check `learning-records/` if any exist.)
- **Constraints.** Deadline, time budget, preferred language, format/pacing prefs.
- **Resources.** Are the shortlisted sources the right ones? Any they trust/avoid?

Rules:
- **Ask one question at a time.** For each, provide your recommended answer.
- If a question can be answered by exploring files/resources, explore instead of asking.
- As decisions settle, **update `.claude/plans/<subject>.md`** so it reflects the
  refined understanding (mission, lesson arc, resources, constraints).

When the plan is sharp, tell the user:
> The plan is solid. Run **`/new-classroom <subject>`** to build the classroom and your first lesson.
