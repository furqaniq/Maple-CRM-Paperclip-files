---
name: cost-attached-reporting
description: Reports per-campaign performance to LEDGER with cost attached, in delivered rather than sent, and with exclusions and halts stated as part of the campaign. Fires at campaign completion, on the reporting cycle, and on demand.
agent: RELAY
division: Marketing
binding: mandate
---

# Cost-Attached Reporting

A campaign that reached a third of its audience is not reported on the third.

## When this fires

- At campaign completion.
- On the scheduled reporting cycle.
- On demand from LEDGER, ATLAS, or the Account Owner.
- Immediately, when a compliance event materially changes what a campaign did — a complaint spike, a filtered send, a suppression failure.

## Inputs

- Per-campaign send, delivery, placement, and engagement counts, kept distinct from one another.
- Cost draw for the campaign from ABACUS's `real-time-consumption-meter`, the account's single cost ledger.
- Test outcomes with their significance stated.
- Exclusion counts by category: consent, frequency suppression, bounce, out-of-window.
- Deliverability incidents, filtering events, and halted sends with their scope.
- Attribution to pipeline, where a traceable basis exists.

## Procedure

1. **Report delivered, not sent.** Accepted-by-carrier and inbox-placed are different facts, and the distinction is the deliverability KPI itself.
2. **Attach cost from ABACUS's meter** — the actual draw, not an estimate — and derive cost per engaged contact from it.
3. **Report exclusions as part of the campaign**, by category and count: who was excluded for consent, for frequency, for bounce, for window. These describe the campaign, not a footnote to it.
4. **Report inconclusive tests as inconclusive**, with the sample reached against the sample planned.
5. **Report deliverability incidents and halted sends with their scope** — what did not go out, to how many, and why.
6. **Deliver to LEDGER with cost attached.** LEDGER interprets; RELAY reports.

## Output

A per-campaign report stating delivered and placed counts separately from sent, cost drawn from the ledger and cost per engaged contact, every exclusion category with its count, test results including inconclusive ones, and every incident and halt with the scope of what did not go out.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from RELAY — these apply to every RELAY skill, per `AGENTS.md` §5:**

- A/B and multivariate winners are declared only on a **real statistical threshold**. An inconclusive test is reported as inconclusive; noise is never dressed as a result.
- **Cross-campaign suppression is enforced** — no contact receives multiple unrelated sends in a day, no matter which campaign or which agent owns each one.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.

**Specific to this skill:**

- **Delivery is reported as inbox-placed or delivered-to-handset, never as accepted-by-carrier.** Reporting acceptance as delivery makes the single number this agent exists to move meaningless.
- **Exclusions and halts appear in the report.** A campaign that reached a third of its audience is reported against its intended audience, not against the third that received it — a rate computed on the reached population hides the failure it is supposed to reveal.
- **RELAY reports; LEDGER interprets and recommends at L1, and neither acts.** RELAY does not present a recommendation as a finding, and a LEDGER recommendation does not become a RELAY action without the human decision that L1 requires.
- **Cost is drawn from ABACUS's meter, never estimated and never recomputed here.** ABACUS holds the single cost ledger for the account; ATLAS enforces budget ceilings against it but does not hold it. A cost figure RELAY could not source is reported as unavailable rather than modeled, and a second cost number derived locally would be the one quoted whenever it was the lower of the two.
- **A compliance event is never omitted or smoothed because it makes the numbers worse.** A complaint spike, a filtered campaign, or a suppression failure appears as itself, at full size, in the period it happened.

## Measured on

Cost per engaged contact · inbox placement · delivery rate · test velocity
