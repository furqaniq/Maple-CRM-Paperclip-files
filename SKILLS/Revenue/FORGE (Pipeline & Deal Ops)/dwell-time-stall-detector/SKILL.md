---
name: dwell-time-stall-detector
description: Flags a file the moment it exceeds expected time in stage, before it becomes a problem rather than after. Runs continuously against every open file.
agent: FORGE
division: Revenue
binding: mandate
---

# Dwell-Time Stall Detector

A stalled file is invisible until it is late, and by then the deadline it was going to miss is already inside the ladder.

## When this fires

- Continuously, the moment a file exceeds its stage's expected dwell time.
- On a file approaching its threshold while a deadline sits behind it.
- On the recalibration cycle for stage-level dwell expectations.

## Inputs

- Every open file, its stage, and its time in that stage.
- Expected dwell per stage, derived from the account's own closed-file history.
- Deadlines behind the file, from [`72-48-24-deadline-ladder`](../72-48-24-deadline-ladder/SKILL.md).
- What the file is actually waiting on — a document, a third party, an internal action.

## Procedure

1. **Derive expected dwell from the account's own history per stage**, not from a generic benchmark.
2. **Flag on crossing the threshold**, not on a daily sweep — the value of the flag decays with every hour.
3. **Name what the file is waiting on**, and who owns it, rather than reporting that it is slow.
4. **Raise the flag's priority where a deadline sits behind the stall**, since a stall in front of an expiring contingency is a different problem from a slow file.
5. **Distinguish a stalled file from a file waiting on a third party under its own clock**, and say which it is.
6. **Route to the owner with the specific next action**, and hand the task to TEMPO's `task-generator`.
7. **Recalibrate expectations against outcomes**, and report where the expectation itself is wrong rather than flagging every file in a stage.

## Output

- A stall flag naming what the file is waiting on and who owns it.
- A raised priority where a deadline sits behind the stall.
- A recalibration finding where a stage's expected dwell no longer matches reality.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from FORGE — these apply to every FORGE skill, per `AGENTS.md` §5:**

- FORGE **never alters terms, rates, locks, or fees.** It surfaces, chases, notifies, and escalates. Changes are human acts with human audit trails.
- Document chasing **always checks the file vault first** before re-requesting from the customer.
- Missed deadlines are a **target-zero** metric — the 72/48/24-hour escalation ladder is mandatory, not optional.

**Specific to this skill:**

- **Detection fires on threshold crossing, never on a schedule.** Stall detection lag is the metric, and a daily sweep sets a floor on it of one day.
- **A flag names the blocker and its owner.** "This file is slow" is not actionable and is ignored by the third one.
- **A stall in front of a deadline is escalated differently.** The deadline ladder is mandatory and target-zero, and a stall that will consume it is the earliest possible warning the ladder is about to be needed.
- **Expected dwell comes from this account's own history.** An industry benchmark flags half the pipeline in a slow market and none of it in a fast one.
- **Waiting on a third party is stated as waiting, not as stalled.** Chasing a producer about a file sitting with an outside appraiser is noise that trains them to dismiss the real ones.
- **A stage flagging most of its files is a calibration finding, not a hundred stall flags.** Flooding the queue is how a genuine stall gets missed.
- **Detection reads file state, never a producer's activity level.** A stall detector that becomes a productivity monitor has changed jobs and belongs to HONE, under HONE's boundary.

## Measured on

Stall detection lag · flags naming a specific blocker and owner · stalls that preceded a missed deadline (target zero) · flag precision after human review
