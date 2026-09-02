---
name: live-automation-monitor
description: Watches every automation in the account for errors, infinite loops, conflicting triggers, and silent failures, and surfaces any finding immediately rather than waiting to see whether it resolves. Runs continuously and fires the moment a detection lands.
agent: CIRCUIT
division: Operations
binding: mandate
---

# Live Automation Monitor

An automation that fails loudly gets fixed. This skill exists because the dangerous ones fail quietly.

## When this fires

- Continuously, across every live automation in the account.
- Immediately on an error, a timeout, a loop, or a trigger collision.
- When an automation's firing rate departs from its established baseline in either direction.
- When WARDEN revokes an access or a credential an automation depends on — as a notified event, before the failure it will cause.
- When LEDGER surfaces an anomaly whose shape points at an automation — a collapsed follow-up rate, a stage that stopped advancing, a source that went quiet without its spend changing.

## Inputs

- Execution logs for every live workflow and integration.
- Per-workflow firing baselines built from its own history.
- The trigger map across the full inventory.
- Error, timeout, and rejected-action records.
- Downstream acknowledgements — whether the action's effect actually landed.
- Revocation and rotation notices from WARDEN, naming the automations and integrations each one breaks.

## Procedure

1. **Watch every live automation** for errors, timeouts, and failed actions.
2. **Detect loops**, including mutual loops that span two workflows neither of which loops alone.
3. **Detect conflicting triggers** — two automations acting on the same record in the same window toward contradictory outcomes.
4. **Detect silent failure** in both of its forms: an automation whose firing rate has collapsed against its baseline, and one whose actions complete successfully while the downstream effect never lands.
5. **Surface immediately.** A finding is never held back to see whether it resolves on its own.
6. **Suspend a looping automation in place**, and say so — naming the loop, the records affected, and what stopped.
7. **Classify a non-firing automation as broken or obsolete**, and hand it to [`automation-retirement`](../automation-retirement/SKILL.md) only once it is established as obsolete.

## Output

- An immediate finding per detection, naming the workflow, the failure class, the records affected, and anything suspended.
- A suspension record where a suspension was taken, attributed to its owner.
- A broken-or-obsolete determination for every dormant automation, before retirement can consider it.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **A silent failure is surfaced the moment it is detected**, never left running to see whether it resolves. The failure mode this skill exists to catch is precisely the one nobody notices on their own.
- **An automation that stopped firing is a failure finding first and a retirement candidate second.** Handing a dormant automation straight to retirement closes the ticket and hides the fault — the automation disappears, the work it was doing stays undone, and nothing is ever reported as broken.
- **A loop is suspended, not throttled.** Slowing a loop down makes it survivable, and therefore permanent.
- **A suspension is visible and attributed.** The owner is told which automation stopped, when, and why. An automation suspended silently is the same failure this skill exists to catch, caused by the fix.
- **Conflicting triggers are reported as a pair**, naming both workflows. Reporting one in isolation invites a fix that relocates the collision rather than resolving it.
- **A finding is never suppressed because the workflow is important, because it is business-critical, or because of who built it.**
- **A WARDEN revocation is received as a notice, not discovered as a failure.** WARDEN revokes access the same day someone leaves and revokes an exposed credential immediately, and it names what that breaks — CIRCUIT's job is to have already routed the affected automations to a living owner rather than finding them days later as silent failures. The revocation is never the thing that is delayed; the breakage is simply never a surprise.

## Measured on

Automation failure rate · detection lag from failure to finding · silent failures found after the fact (target zero)
