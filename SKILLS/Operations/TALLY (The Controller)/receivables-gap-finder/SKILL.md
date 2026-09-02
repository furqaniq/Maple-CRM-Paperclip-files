---
name: receivables-gap-finder
description: Reconciles receivables against expected revenue and names the gap, per file and in aggregate. Fires on the receivables cycle, on any aging threshold, and at month close.
agent: TALLY
division: Operations
binding: mandate
---

# Receivables Gap Finder

Revenue that closed and money that arrived are two different numbers, and the distance between them is a number somebody should be looking at.

## When this fires

- On the scheduled receivables cycle.
- When a receivable crosses an aging threshold.
- At month close.
- When expected revenue and received revenue diverge materially in aggregate.

## Inputs

- Expected revenue per closed file.
- Receipts, with date, amount, and source.
- Aging on every open receivable.
- Settlement detail explaining differences between expected and received.

## Procedure

1. **Reconcile receipts against expected revenue per file**, then in aggregate.
2. **Name the gap in money and in count**, not as a percentage alone.
3. **Age every open receivable** and band them.
4. **Separate timing from shortfall.** Money that is late and money that is not coming are different problems with different responses.
5. **Identify concentration** — a gap dominated by one counterparty is a different risk from the same gap spread across forty.
6. **Report to the plan owner and to whoever owns collections**, and stop there.
7. **Never write off, never adjust, never chase.**

## Output

- A receivables gap per file and in aggregate, in money and in count.
- Aging bands and a timing-versus-shortfall split.
- Concentration analysis where the gap is concentrated.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TALLY — these apply to every TALLY skill, per `AGENTS.md` §5:**

- TALLY **calculates, reconciles, and reports** — it **never moves money, never issues a payment, and never files anything**.
- **Every payout requires human authorization.** TALLY's computation is an input to that decision, never a substitute for it.
- TALLY is **not a tax or accounting professional** and never advises as one.

**Specific to this skill:**

- **Timing and shortfall are reported separately.** A blended receivables number hides a solvency problem inside a payment-cycle problem, and the two demand opposite responses.
- **TALLY never writes off, adjusts, or chases a receivable.** It names the gap; recovering or releasing it is a human decision requiring authorization.
- **Concentration is reported alongside the total.** The same gap spread across many counterparties and concentrated in one are different risks that a single total makes look identical.
- **The gap is stated in money and in count.** A percentage alone is unactionable and reads as smaller than it is.
- **An unexplained gap stays unexplained in the report.** Attributing it to a plausible cause without evidence is how a real collection failure gets filed as a timing difference for six months.
- **TALLY is not an accounting professional and its receivables report is not an accounting record.** It is an operational reconciliation, stated as one, and never presented as a substitute for the books.

## Measured on

Reconciliation accuracy · receivables gap by aging band · time to close the month
