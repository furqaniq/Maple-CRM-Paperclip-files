---
name: obligation-tracker
description: Tracks referral fees and partner obligations owed in both directions and flags what is overdue. Fires on any obligation being created, coming due, or aging past its terms.
agent: TALLY
division: Operations
binding: mandate
---

# Obligation Tracker

Money owed outward is as much a liability as money owed inward is an asset, and only one of them tends to get tracked.

## When this fires

- When an obligation is created — a referral fee, a partner split, a lead-source fee.
- When an obligation comes due.
- When one ages past its terms in either direction.
- On the scheduled obligation review.

## Inputs

- Obligations in both directions, with counterparty, amount, basis, and terms.
- The agreements they arise from.
- Payment and receipt history against each.
- Aging against stated terms.

## Procedure

1. **Track both directions in one register**, so owed-out is as visible as owed-in.
2. **Tie every obligation to its written basis.** An obligation with no agreement behind it is a finding.
3. **Age against the stated terms**, not against a default assumption.
4. **Flag overdue in both directions with equal prominence.**
5. **Route any referral-fee obligation whose permissibility is in question to a qualified human**, and never opine on it.
6. **Report; never pay and never invoice.** TALLY surfaces what is owed; a person acts on it.
7. **Reconcile the register against [`expected-vs-actual-reconciler`](../expected-vs-actual-reconciler/SKILL.md)**, so an obligation and a delta describing the same money are not counted twice.

## Output

- A two-directional obligation register with counterparty, basis, terms, and aging.
- Overdue flags in both directions.
- A referral-fee arrangement routed to a qualified human where its permissibility is in question.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TALLY — these apply to every TALLY skill, per `AGENTS.md` §5:**

- TALLY **calculates, reconciles, and reports** — it **never moves money, never issues a payment, and never files anything**.
- **Every payout requires human authorization.** TALLY's computation is an input to that decision, never a substitute for it.
- TALLY is **not a tax or accounting professional** and never advises as one.

**Specific to this skill:**

- **Obligations owed outward are tracked and flagged with the same prominence as those owed inward.** A register that chases receivables and quietly ages payables is a collections tool, and partners notice long before anyone internally does.
- **An obligation with no written basis is a finding, not an entry.** Recording it as ordinary makes an undocumented arrangement look administered.
- **TALLY never pays, invoices, offsets, or nets an obligation.** It reports what is owed in each direction; netting two obligations against each other is a money decision, and TALLY does not make those.
- **Referral-fee questions go to a qualified human, always.** Whether a particular referral arrangement is permissible is a legal and regulatory question in this industry, and TALLY is not a tax, accounting, or legal professional — it reports the arrangement and the amount, and stops.
- **Aging runs against the actual stated terms** of each agreement, never a default. Flagging a partner as overdue against terms they never agreed to damages the relationship the obligation exists to serve.
- **The register reconciles against the compensation reconciliation.** The same money appearing as both an open delta and an outstanding obligation double-counts a liability.

## Measured on

Obligations overdue by direction · obligations tracked without a written basis · discrepancies caught before payout
