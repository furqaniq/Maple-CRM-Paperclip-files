---
name: historical-backtest
description: Replays a new or modified workflow against historical data and reports what would have happened before activation. This is the only skill that activates a workflow, and it never activates one without a report a human approved.
agent: CIRCUIT
division: Operations
binding: mandate
---

# Historical Backtest

The gate between a workflow that looks right and a workflow that is right. Nothing activates without passing through here.

## When this fires

- Whenever a workflow reaches the end of compilation.
- Whenever a live workflow is structurally modified.
- On request, before a retired automation is brought back into service.

## Inputs

- The inactive workflow definition.
- The account's historical event stream over a window long enough to be representative.
- The records the workflow would have touched in that window.
- The current live automation inventory, for interaction effects.

## Procedure

1. **Replay the workflow against the historical window**, recording every record it would have touched and every action it would have taken.
2. **Report in outcome terms** — how many contacts, how many sends, how many stage changes, how many records altered — not how many nodes executed.
3. **Name the surprises**: records the user would not expect to be caught, volumes larger than the description implied, branches that never fired at all.
4. **Where the history is thin or absent, say so explicitly.** A backtest with nothing to test against is a failed backtest, not a passed one.
5. **Present the result to the user for an activation decision.**
6. **Activate only on that approval**, and write the activation to the audit trail with the backtest report attached.

## Output

- A backtest report: records touched, actions taken, branch coverage, and named surprises.
- An activation decision, made by a human.
- On activation, an audit entry carrying the report that justified it.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **No workflow activates without a backtest.** Not for an urgent request, not for a small change, not for a workflow the user is confident about, and not as a temporary measure that will be reviewed later.
- **Absent history is a fail, not a pass.** A workflow that cannot be backtested because no comparable history exists activates only under an explicit, recorded human decision that names the missing evidence — never by default because the report came back empty.
- A backtest that would have touched **more records than the description implied is surfaced as a surprise**, never buried inside a count the reader has to interpret.
- The backtest runs against **real historical data**, never a synthetic or hand-picked sample that makes the workflow look correct.
- **A structural modification re-enters the backtest.** Editing a live workflow is not exempt because it is already running — editing around the gate is the same as skipping it.
- **The backtest never activates on its own confidence.** It reports; a human approves. A clean report is not an approval.

## Measured on

Automation failure rate · workflows activated without a backtest (target zero) · surprises caught pre-activation
