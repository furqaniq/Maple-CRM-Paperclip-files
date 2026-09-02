---
name: overdue-chaser
description: Chases overdue tasks and escalates specifically the ones blocking money, rather than letting them age out in the standard queue. Fires as each task passes due and at each escalation interval after.
agent: TEMPO
division: Revenue
binding: mandate
---

# Overdue Chaser

Every overdue task is chased; the ones blocking money are escalated, and the difference between those two words is the whole skill.

## When this fires

- As each task passes its due date.
- At each configured escalation interval after that.
- Immediately on a money-blocking task passing due, without waiting for the first interval.

## Inputs

- The overdue task, its owner, its age, and its money- or deadline-blocking flag.
- The owner's live availability and current load.
- The escalation path from WARDEN's `org-structure-manager`.
- FORGE's deadline state on the file, where the task sits on one.

## Procedure

1. **Chase the owner directly first**, with what is overdue and what it is blocking.
2. **Escalate a money-blocking task immediately on passing due**, rather than at the first standard interval.
3. **Escalate up the real reporting path**, not to a shared queue.
4. **Leave file deadlines to FORGE's 72/48/24 ladder** and chase only the task attached to them, so a single deadline does not produce two independent escalation ladders reaching the same person.
5. **Reassign rather than keep chasing an owner who is unavailable** — on leave, at capacity, or departed.
6. **Escalate the pattern, not just the item**, where one owner has many overdue tasks, since the fix is a workload problem rather than a reminder problem.
7. **Stop chasing a task whose obligation no longer exists**, and close it with the reason.

## Output

- A chase to the owner naming what is overdue and what it blocks.
- An immediate escalation on money-blocking tasks, up the real reporting path.
- A reassignment or a pattern escalation where chasing the individual is not the fix.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TEMPO — these apply to every TEMPO skill, per `AGENTS.md` §5:**

- No-show recovery attempts happen **within minutes**, never deferred to the next day.
- Every auto-generated task carries a **real owner, a real due date, and context** — never a bare reminder with no accountable party.
- Tasks blocking money are **escalated**, not left in the standard queue to age out.

**Specific to this skill:**

- **A money-blocking task escalates on passing due, not at the standard interval — this is TEMPO's own boundary.** Aging out in the standard queue is exactly what the rule forbids, and the standard queue is where it happens by default.
- **TEMPO owns tasks; FORGE owns file deadlines.** A file deadline runs FORGE's 72/48/24 ladder, and TEMPO chases only the task hanging off it. Two agents laddering the same deadline produce six notifications about one thing, and the recipient learns to ignore all of them — including the ones from the agent that was right.
- **Escalation follows the real reporting path.** A shared queue is not an escalation; it is a second place for the same item to sit.
- **An unavailable owner is reassigned, not chased.** Chasing someone on leave produces a perfect escalation record and no completed work.
- **A repeated pattern escalates as a pattern.** Twenty overdue tasks on one person is not twenty reminder failures, and treating it as one is how a person's workload problem becomes their performance record.
- **A chase never reaches the customer.** An overdue internal task is an internal matter, and messaging a contact about the company's own delay is a decision a human makes.
- **A task whose obligation has gone is closed with its reason, not chased into silence.** Chasing dead tasks is the fastest way to make the whole queue ignorable.

## Measured on

Task completion rate · money-blocking tasks escalated on passing due (must be 100%) · duplicate escalation ladders with FORGE (target zero) · overdue tasks on departed or unavailable owners
