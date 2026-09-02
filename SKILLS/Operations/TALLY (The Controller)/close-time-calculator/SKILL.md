---
name: close-time-calculator
description: Computes expected compensation per file at the moment it closes, under the plan rules in force on that date. Fires on every close and on any correction to a closed file.
agent: TALLY
division: Operations
binding: mandate
---

# Close-Time Calculator

Compensation computed at close, not reconstructed three weeks later from memory and a spreadsheet.

## When this fires

- On every file close.
- On a correction to a file already closed.
- On a plan version change with retroactive effect, to recompute the affected period under the correct rules.
- When a file closes under a plan whose rules do not cleanly cover it.

## Inputs

- The closed file: revenue, product, source, participants, and close date.
- The plan version in force on that close date, from [`comp-structure-modeler`](../comp-structure-modeler/SKILL.md).
- Period-to-date position against tiers and caps.
- Overrides, splits, and fees attaching to the file.

## Procedure

1. **Use the plan version in force on the close date**, never the current one.
2. **Compute every component separately** — base split, tier effect, cap effect, override, referral fee, branch split — and show each rather than a single figure.
3. **Update the period-to-date tier and cap position**, since this file may move the next one's rate.
4. **Show the computation, not just the result.** A producer who cannot follow the arithmetic will dispute it.
5. **Flag rather than resolve where the rules do not cleanly cover the file**, and route it to the plan owner.
6. **Publish to [`producer-live-view`](../producer-live-view/SKILL.md) immediately**, so the producer sees it at close rather than at statement.
7. **Mark the figure as expected, not received**, until reconciliation confirms it.

## Output

- An expected compensation figure per file, itemized by component.
- A visible computation the producer can follow.
- An updated period-to-date tier and cap position.
- A flag where the rules did not cleanly cover the file.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TALLY — these apply to every TALLY skill, per `AGENTS.md` §5:**

- TALLY **calculates, reconciles, and reports** — it **never moves money, never issues a payment, and never files anything**.
- **Every payout requires human authorization.** TALLY's computation is an input to that decision, never a substitute for it.
- TALLY is **not a tax or accounting professional** and never advises as one.

**Specific to this skill:**

- **Expected is never presented as received.** The figure this skill produces is a computation; whether the money arrived is [`expected-vs-actual-reconciler`](../expected-vs-actual-reconciler/SKILL.md)'s answer, and blurring the two is how a producer plans around money that never came.
- **The plan version in force on the close date governs**, without exception. Recomputing an old file under a new plan silently changes what someone earned.
- **Every component is shown separately.** A single number is a number to argue with; an itemized computation is a number to check.
- **A file the rules do not cleanly cover is flagged, never approximated.** TALLY produces exact arithmetic on stated rules; when the rules run out, so does TALLY's authority.
- **TALLY computes; it never authorizes.** A close-time calculation is an input to a payout decision, never the decision, and it never initiates anything downstream.
- **A correction is a new computation with the original preserved.** Overwriting a producer's earlier figure without a trace is how trust in the whole system goes.

## Measured on

Reconciliation accuracy · producer disputes raised · files flagged rather than approximated
