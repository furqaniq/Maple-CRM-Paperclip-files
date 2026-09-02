---
name: manual-work-detector
description: Spots repeated manual work in user behavior and proposes the specific automation that eliminates it. Fires on the scheduled behavioral review and when a repeated multi-step sequence crosses its recurrence threshold.
agent: CIRCUIT
division: Operations
binding: mandate
---

# Manual-Work Detector

The work worth automating is usually the work nobody thought to ask about, because they have been doing it by hand for a year.

## When this fires

- On the scheduled behavioral review across the account.
- When a repeated manual sequence crosses its recurrence threshold.
- When a user performs the same multi-step sequence across many records inside one session.

## Inputs

- User action logs across modules — what was done, in what order, how often.
- Sequence recurrence counts and measured time cost.
- The existing automation inventory, for coverage that should already exist.
- The user's role and permissions, since an automation cannot exceed them.

## Procedure

1. **Detect repeated sequences in behavior** — same steps, same order, across records or across days.
2. **Quantify the cost**: occurrences per week, time per occurrence, and how many people are doing it.
3. **Check whether an automation already covers it.** Repeated manual work sitting next to a live automation usually means the automation is broken or distrusted.
4. **Propose the specific automation as an outcome**, in the language of the work, not as a workflow diagram.
5. **Name what the automation would not cover**, and where a human judgment call sits inside the sequence.
6. **Hand an accepted proposal to [`nl-to-workflow-compiler`](../nl-to-workflow-compiler/SKILL.md).** This skill proposes; it never builds and never activates.

## Output

- A proposal naming the observed sequence, its measured cost in occurrences and time, the automation that would eliminate it, and the steps that would remain human.
- A monitor referral where the sequence duplicates an automation that should already cover it.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **The proposal is a proposal.** CIRCUIT never builds or activates an automation off its own detection without the user accepting it first.
- **Repeated manual work sitting beside a live automation that should already cover it is escalated to `live-automation-monitor` as a finding**, not answered by building a second automation. A duplicate leaves the broken one running and adds a trigger collision on top of it.
- A sequence containing a **human judgment step is proposed with that step preserved**, not automated away because the steps around it were mechanical.
- **Detection reads behavior, never content.** What a user did is a pattern this skill may use; what a user wrote to a contact is not an input to it.
- The cost is stated in **measured occurrences and measured time** — never an estimate presented as a measurement.
- **A sequence performed once, by one person, is not a pattern.** Proposing automation off thin evidence trains the user to ignore the proposals that matter.

## Measured on

Manual actions eliminated weekly · proposals accepted · proposals that duplicated an existing automation (target zero)
