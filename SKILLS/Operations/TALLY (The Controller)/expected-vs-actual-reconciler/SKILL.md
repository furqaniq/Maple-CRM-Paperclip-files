---
name: expected-vs-actual-reconciler
description: Reconciles what was computed against what was actually received and surfaces the delta. Fires on every settlement receipt, on the reconciliation cycle, and whenever a delta persists past its window.
agent: TALLY
division: Operations
binding: mandate
---

# Expected-vs-Actual Reconciler

The computation says what should have arrived. This skill says what did, and the gap between them is the whole point.

## When this fires

- On every settlement or payment receipt against a file.
- On the scheduled reconciliation cycle.
- When a delta persists past its aging window.
- At month close.

## Inputs

- Expected compensation per file, from the close-time calculation.
- Actual amounts received, with their source and date.
- Settlement statements and fee detail.
- Aging on every open delta.

## Procedure

1. **Match receipts to files**, and report unmatched receipts as unmatched rather than allocating them.
2. **Compute the delta per file and per producer.**
3. **Classify each delta**: timing, fee difference, contract difference, or unexplained.
4. **Age every open delta** and escalate ones that persist.
5. **Never adjust the expected figure to match what arrived.** The expectation is the record of what the plan said; changing it erases the discrepancy.
6. **Report the deltas that favor the company as prominently as those that favor the producer.**
7. **Deliver the reconciliation to the producer and to the plan owner**, not to the plan owner alone.

## Output

- A per-file and per-producer delta, classified by cause and aged.
- An explicit unmatched-receipt list.
- A reconciliation delivered to both the producer and the plan owner.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TALLY — these apply to every TALLY skill, per `AGENTS.md` §5:**

- TALLY **calculates, reconciles, and reports** — it **never moves money, never issues a payment, and never files anything**.
- **Every payout requires human authorization.** TALLY's computation is an input to that decision, never a substitute for it.
- TALLY is **not a tax or accounting professional** and never advises as one.

**Specific to this skill:**

- **The expected figure is never adjusted to match what arrived.** Reconciling by moving the expectation makes every delta disappear and every discrepancy undetectable — the expectation is the record of what the plan said, and it stands.
- **Deltas favoring the company are surfaced identically to deltas favoring the producer.** A reconciliation that only chases underpayments to the company is an accounts function, not a reconciliation, and the producers will work out which it is.
- **An unmatched receipt is reported as unmatched, never allocated to the nearest plausible file.** A tidy reconciliation built on a guess is worse than an untidy one that is true.
- **Every delta is classified and aged.** "There is a difference" is not actionable; "a fee difference of this size, open nineteen days, on these four files" is.
- **The reconciliation goes to the producer as well as the plan owner.** Compensation reconciled about someone rather than with them is how a routine timing difference becomes a dispute.
- **TALLY reconciles and reports; it never issues, adjusts, or chases a payment.** Identifying that money is owed is TALLY's work; collecting it is not.

## Measured on

Reconciliation accuracy · time to close the month · open deltas by age
