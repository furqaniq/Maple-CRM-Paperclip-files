---
name: producer-live-view
description: Shows each producer their own pipeline value, earned-to-date, and progress toward the next tier, live rather than at month end. Fires on every file event that changes a producer's position.
agent: TALLY
division: Operations
binding: mandate
---

# Producer Live View

Visibility changes behavior. That is the point of this skill, and it is also the reason it has to be scoped tightly.

## When this fires

- On every file event that changes a producer's expected compensation or tier position.
- On every reconciliation that changes what is confirmed.
- On a plan change affecting the producer.
- Whenever the producer opens their own view.

## Inputs

- The producer's own files, expected compensation, and confirmed receipts.
- Their tier and cap position for the period.
- Their pipeline, with expected value and its uncertainty.
- Their permission scope from WARDEN, which bounds what the view may contain.

## Procedure

1. **Show their own numbers only**, bounded by WARDEN's permission model rather than by TALLY's convenience.
2. **Separate expected from confirmed** visually and unambiguously.
3. **Show tier progress with the gap named** — what closes the gap, in files or in volume.
4. **Show pipeline value with its uncertainty attached**, never as a figure to plan around.
5. **Surface open deltas on their own files**, so a producer is not the last to know about a discrepancy in their own compensation.
6. **Update live**, so the view and the statement never disagree.
7. **Never rank.** A producer sees their own position, not their position relative to anyone else.

## Output

- A live per-producer view: expected, confirmed, tier position, pipeline value, and open deltas.
- A named gap to the next tier.
- Expected and confirmed shown as distinct states.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TALLY — these apply to every TALLY skill, per `AGENTS.md` §5:**

- TALLY **calculates, reconciles, and reports** — it **never moves money, never issues a payment, and never files anything**.
- **Every payout requires human authorization.** TALLY's computation is an input to that decision, never a substitute for it.
- TALLY is **not a tax or accounting professional** and never advises as one.

**Specific to this skill:**

- **A producer sees their own numbers and no one else's.** Team overrides and branch splits mean one file's compensation involves several people — the view shows the producer's own component, never the counterparties' amounts, and WARDEN's permission model decides that, not TALLY.
- **Where WARDEN's model has no answer, the view shows the producer's own component only.** TALLY fails closed rather than defaulting to what is convenient to render — an unresolvable permission question is never resolved in favor of showing more, and the gap is reported to WARDEN as a missing role definition.
- **Expected and confirmed are visually distinct and never blended.** A producer planning against an expected figure that has not been received is exactly the harm real-time visibility can cause instead of prevent.
- **The view never ranks producers against each other.** LEDGER's coaching scorecard already forbids leaderboards for good reasons; a compensation leaderboard is the same failure with money attached, and it would arrive from an agent nobody thought to check.
- **Pipeline value carries its uncertainty.** A pipeline figure shown as a number gets treated as earnings, and a producer makes a personal financial decision on a deal that has not closed.
- **Open deltas on a producer's own files are visible to that producer.** Discovering a discrepancy in your own pay from someone else, weeks later, is how disputes become grievances.
- **The live view and the statement are the same numbers.** Two surfaces disagreeing about someone's pay destroys confidence in both.

## Measured on

Producer disputes raised · view-to-statement agreement (target 100%) · cross-producer data exposure (target zero)
