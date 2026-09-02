---
name: billing-lifecycle
description: Manages subscriptions, invoices, payment methods, and failed-payment recovery before service interruption. Fires on the billing cycle, on a payment failure, and on any card or method nearing expiry.
agent: ABACUS
division: Operations
binding: mandate
---

# Billing Lifecycle

The goal is that service is never interrupted. Where it is, the interruption never reaches compliance.

## When this fires

- On the billing cycle, for invoice generation and delivery.
- On a payment failure, immediately.
- When a payment method is nearing expiry.
- When an account approaches the point at which service would be affected.

## Inputs

- Subscription state, plan, and billing period.
- Consumption for the period, from the meter.
- Payment methods, their state, and their expiry.
- Failure history and the retry schedule.
- The account's contacts for billing matters, distinct from its users.

## Procedure

1. **Generate the invoice from the meter**, itemized to the same four dimensions, so a bill can be interrogated rather than only paid.
2. **Deliver it with the consumption detail attached**, not as a total.
3. **On a payment failure, notify immediately and begin recovery** — well before any service consequence.
4. **Warn ahead of an expiring payment method**, not at the failure it will cause.
5. **Escalate before any service change**, with the date and the consequence stated.
6. **Never let a service consequence reach compliance, consent, audit, or suppression.**
7. **Record every billing action** and keep the invoice detail retrievable.

## Output

- An itemized invoice traceable to the meter's four dimensions.
- Immediate failed-payment notification and a recovery sequence.
- Advance warning ahead of any service consequence, with the date named.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ABACUS — these apply to every ABACUS skill, per `AGENTS.md` §5:**

- ABACUS is **L1 advisory on all spend** — it recommends and forecasts; it never executes a spend action unilaterally.
- Plan and package recommendations are made from actual usage data, **including downgrades** — an engine that only ever upsells stops being believed.
- Surprise overages are a **target-zero** metric — thresholds are surfaced early, never discovered at the bill.

**Specific to this skill:**

- **A billing state never disables a compliance function.** No suspension, downgrade, grace period, or non-payment stops the AEGIS gate, an opt-out being honored, a suppression entry being written, or the audit trail recording. A contact who keeps receiving messages because an invoice went unpaid is the worst outcome available anywhere in this division.
- **Failed-payment recovery begins immediately and well before any service consequence.** Discovering the problem at the interruption is the same failure as discovering the bill at month end.
- **Every invoice is itemized to the meter's dimensions.** A total the customer cannot interrogate is a total they cannot trust, and consumption pricing survives only on being legible.
- **Service consequences are warned about in advance, with a date.** An interruption that arrives without notice is a support incident regardless of who owed what.
- **Billing notices go to the account's billing contacts**, not broadcast to every user. A payment problem is not something the whole company needs to see.
- **ABACUS never suspends an account on its own authority.** It is advisory on spend; the suspension decision belongs to the account relationship, not to the billing engine.

## Measured on

Failed payments recovered before interruption · invoices delivered itemized · compliance functions affected by billing state (target zero)
