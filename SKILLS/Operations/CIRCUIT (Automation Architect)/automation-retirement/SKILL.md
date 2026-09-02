---
name: automation-retirement
description: Retires automations that no longer fire or no longer matter, but only after the monitor has established the automation is obsolete rather than broken. Fires on the scheduled inventory review, on a cleared dormancy finding, and when a process is discontinued or superseded.
agent: CIRCUIT
division: Operations
binding: mandate
---

# Automation Retirement

Dead automations are not harmless. They collide with live ones, hold credentials, and hide the work that quietly stopped happening.

## When this fires

- On the scheduled automation inventory review.
- When an automation has been dormant past its window and [`live-automation-monitor`](../live-automation-monitor/SKILL.md) has cleared it as obsolete rather than broken.
- When the business process a workflow served is discontinued.
- When a workflow is superseded by a replacement.

## Inputs

- Firing history per workflow.
- The monitor's determination — obsolete or broken.
- Every downstream dependency: chained workflows, integrations, reports, and the fields it maintained.
- The workflow's owner and its audit trail.

## Procedure

1. **Confirm with `live-automation-monitor` that the automation is dormant because it is obsolete, not because it is failing.** A broken automation is never retired as a dead one.
2. **Identify everything that depends on it** — chained workflows, integrations, reports, and any field it was responsible for maintaining.
3. **Name what stops happening** when it is retired, and confirm with the owner that this is intended.
4. **Deactivate first and hold through an observation window** before removal, so a wrong call stays reversible.
5. **Preserve the definition, the documentation, and the execution history.**
6. **Release the credentials and integration references it held** back to WARDEN.
7. **Write the audit entry**: what was retired, who approved it, and what stopped happening as a result.

## Output

- A deactivated and then removed automation.
- A retirement record naming dependencies, the work that stopped, and the approving human.
- Released credential and integration references, returned to WARDEN.
- A preserved definition, documentation, and execution history.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **A broken automation is never retired as an obsolete one.** Dormancy has two causes that demand opposite responses; retiring a failure closes the ticket while the work it was doing stays permanently undone.
- **Retirement is deactivate-then-observe, never immediate deletion.** A retirement that turns out to be wrong has to be reversible on the day someone notices.
- **The definition, documentation, and execution history are preserved.** Retirement removes an automation from service; it never removes the record of what ran against real contacts.
- **Dependencies are named before retirement, not discovered after.** A retired workflow that another workflow was chained to fails that chain silently — the exact failure class CIRCUIT exists to prevent.
- **What stops happening is stated plainly** and confirmed by the owner. "It never fires" and "nobody noticed it stopped mattering" are different findings with different answers.
- **Credentials and integration references are released back to WARDEN**, never left held by a retired workflow.

## Measured on

Dormant automations resolved correctly (broken versus obsolete) · retirements reversed · credentials released on retirement
