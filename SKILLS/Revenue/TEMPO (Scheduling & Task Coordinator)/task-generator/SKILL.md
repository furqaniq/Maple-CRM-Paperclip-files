---
name: task-generator
description: Creates tasks from calls, conversations, stage changes, and deadlines with a real owner, a real due date, and context attached. Fires on every event that produces an obligation.
agent: TEMPO
division: Revenue
binding: mandate
---

# Task Generator

A task with no owner and no date is a note, and notes are where obligations go to be forgotten.

## When this fires

- On every commitment captured by VOX's `post-call-structurer` or ECHO's `structured-writeback`.
- On a stage change, a deadline, or a condition from FORGE.
- On any agent producing an obligation that needs a human to complete it.

## Inputs

- The originating event and the obligation it created.
- The current owner set from WARDEN's `org-structure-manager`, so an owner is a live seat.
- The relevant record, file, and conversation context.
- Whether the obligation blocks money or a deadline.

## Procedure

1. **Name a real owner from the live seat set.** An owner is a person, never a team, a queue, or a role nobody occupies.
2. **Set a real due date derived from the obligation**, not a default offset applied because a date was required.
3. **Attach the context the owner needs to act** — the record, what was promised, by whom, and to whom — so the task is completable without hunting for it.
4. **Mark whether it blocks money or a deadline**, for [`overdue-chaser`](../overdue-chaser/SKILL.md).
5. **Deduplicate against open tasks on the same obligation**, rather than creating a second one each time the event repeats.
6. **Reassign rather than orphan** where the named owner leaves or changes role.
7. **Close the task on evidence the obligation was met**, not on a stage change that merely implies it.

## Output

- A task with a named live owner, a derived due date, and attached context.
- A money-blocking or deadline-blocking flag where it applies.
- A reassignment where the owner is no longer a live seat.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TEMPO — these apply to every TEMPO skill, per `AGENTS.md` §5:**

- No-show recovery attempts happen **within minutes**, never deferred to the next day.
- Every auto-generated task carries a **real owner, a real due date, and context** — never a bare reminder with no accountable party.
- Tasks blocking money are **escalated**, not left in the standard queue to age out.

**Specific to this skill:**

- **Every task has a real owner and a real due date — this is TEMPO's own boundary.** A task on a team, a queue, or an unfilled role is a bare reminder with no accountable party, which is precisely what the rule forbids, and it is the form the rule is most often broken in.
- **The owner is checked against live seats.** A task assigned to someone WARDEN revoked this morning is invisible work, and the customer commitment behind it is still outstanding.
- **The due date is derived from the obligation, never a default.** A seven-day default on a promise made for tomorrow is a missed promise with a green task beside it.
- **Context is attached, not referenced.** A task that requires reconstructing the conversation before it can be started is a task that gets deferred.
- **Tasks are deduplicated against the open set.** A repeated trigger producing four identical tasks trains the owner to ignore the list, and the one that mattered is in it.
- **A task closes on evidence, not on inference.** A stage advancing does not mean the document arrived, and closing on the stage change hides exactly the gap FORGE exists to catch.
- **A task is never created to record a decision an agent is not allowed to make.** "Confirm the customer's approval" as a task is an eligibility outcome routed through a to-do list.

## Measured on

Task completion rate · tasks created without a live owner or a derived due date (target zero) · duplicate tasks on one obligation · tasks orphaned by a departure (target zero)
