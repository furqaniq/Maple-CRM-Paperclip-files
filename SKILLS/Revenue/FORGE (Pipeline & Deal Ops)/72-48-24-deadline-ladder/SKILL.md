---
name: 72-48-24-deadline-ladder
description: Escalates expirations, contingencies, and inspection windows at three fixed intervals rather than once. Fires at 72, 48, and 24 hours before every tracked deadline, and immediately on a deadline discovered inside the window.
agent: FORGE
division: Revenue
binding: mandate
---

# 72/48/24 Deadline Ladder

One notification at 72 hours is a notification somebody was on holiday for; three is a ladder, and the ladder is mandatory.

## When this fires

- At 72, 48, and 24 hours before every tracked deadline on every open file.
- Immediately, at every remaining rung, on a deadline discovered inside the window.
- On a deadline changing date, which resets the ladder against the new date.

## Inputs

- Every tracked deadline — expirations, contingencies, inspection windows, rate locks, document expiries.
- The file, its owner, and the escalation path from WARDEN's `org-structure-manager`.
- What action would satisfy the deadline, and who can take it.
- Acknowledgement state on each rung already sent.

## Procedure

1. **Track every deadline on the file**, including ones arriving through documents and third parties rather than through the pipeline.
2. **Escalate at all three rungs**, to a progressively wider set — the owner, then their manager, then the account's configured escalation target.
3. **Name the specific action that satisfies the deadline** at every rung, not just the date.
4. **Fire every remaining rung immediately** where a deadline is discovered inside 72 hours, rather than skipping to the next scheduled one.
5. **Reset the ladder against the new date** when a deadline moves, and state that it moved.
6. **Escalate the missed deadline itself** as an incident when a rung passes with the deadline unmet, rather than closing the ladder quietly.
7. **Never alter, extend, or waive a deadline** — see [`terms-interlock`](../terms-interlock/SKILL.md).

## Output

- Three escalations per deadline, each naming the satisfying action and reaching a progressively wider set.
- An immediate multi-rung fire on a deadline discovered inside the window.
- An incident record on any deadline that passed unmet.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from FORGE — these apply to every FORGE skill, per `AGENTS.md` §5:**

- FORGE **never alters terms, rates, locks, or fees.** It surfaces, chases, notifies, and escalates. Changes are human acts with human audit trails.
- Document chasing **always checks the file vault first** before re-requesting from the customer.
- Missed deadlines are a **target-zero** metric — the 72/48/24-hour escalation ladder is mandatory, not optional.

**Specific to this skill:**

- **The ladder is mandatory, not optional — this is FORGE's own boundary.** All three rungs fire. Suppressing a rung because the owner acknowledged the previous one is how a ladder becomes a single notification, and acknowledgement is not the same as the action being taken.
- **A deadline discovered inside the window fires every remaining rung immediately.** Waiting for the next scheduled rung on a deadline found at 30 hours means the ladder delivers one notification, which is the design this skill exists to replace.
- **Every rung names the action that satisfies the deadline.** A date with no action is a reminder that something bad is coming and no instruction for preventing it.
- **FORGE never alters, extends, or waives a deadline.** It surfaces, chases, notifies, and escalates. A deadline moved by an agent is a term altered by an agent.
- **A moved deadline resets the ladder and says it moved.** A silently reset ladder means the recipient's mental model of the timeline is wrong and nothing corrected it.
- **A missed deadline is an incident, and the ladder does not close quietly.** Missed deadlines are a target-zero metric, and a target-zero metric that closes its own alerts stops being measured.
- **TEMPO's `overdue-chaser` chases the task; this skill escalates the deadline.** Two ladders on one date produce six notifications about one thing and teach the recipient to dismiss all of them.

## Measured on

Missed deadlines (target zero) · rungs fired as a share of rungs due (must be 100%) · deadlines discovered inside the window · duplicate ladders with TEMPO (target zero)
