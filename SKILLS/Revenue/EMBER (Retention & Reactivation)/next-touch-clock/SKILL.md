---
name: next-touch-clock
description: Holds a next-touch date and a stated reason for every dormant contact, so the database is a worked asset rather than a stored one. Fires on every contact entering dormancy and on every touch that resets the clock.
agent: EMBER
division: Revenue
binding: mandate
---

# Next-Touch Clock

A date with no reason produces a call with nothing to say, and that call is why databases stop being worked.

## When this fires

- On every contact entering the dormant pool, including on FORGE's `close-handoff`.
- On every touch, which resets the clock and requires a new reason.
- On a contact whose clock has run out with no reason available.
- On a reason expiring before its date arrives.

## Inputs

- The dormant contact, their record, and the per-contact brief from ATLAS's `memory-brief-store`.
- The handoff record from FORGE, including position as recorded, key dates, and trigger conditions.
- Channel consent and permanent exit state from AEGIS's `consent-ledger`.
- Candidate reasons from [`benefit-math-trigger`](../benefit-math-trigger/SKILL.md) and the contact's own stated timeline.

## Procedure

1. **Accept the handoff explicitly on close**, so no file sits between FORGE and EMBER unowned.
2. **Set a next-touch date and a stated reason together.** Neither is set without the other.
3. **Derive the reason from the record** — an anniversary, an eligibility window, a stated timeline, a market condition — never from the fact that time has passed.
4. **Re-derive the reason as the date approaches**, and defer the touch where the reason has expired rather than reaching out anyway.
5. **Check the twenty-one-day guard and the contact's consent state before the touch**, not after composing it.
6. **Reset the clock and require a new reason after every touch**, including touches made by other agents.
7. **Surface a contact with no available reason** rather than manufacturing one or dropping them from the pool.

## Output

- A next-touch date and a stated reason on every dormant contact.
- A deferral where the reason expired before the date arrived.
- A surfaced contact where no genuine reason exists.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **No touch without a stated, record-grounded reason.** "It has been ninety days" is not a reason, it is a calendar, and a calendar-driven database is the one that produces unsubscribes rather than opportunities.
- **The reason is re-derived at the date, not assumed from when it was set.** A reason that made sense three months ago and no longer holds produces the exact conversation that ends a relationship — the company calling about something that is not true any more.
- **A contact with no reason is surfaced, never manufactured into one and never quietly dropped.** Both failure modes are invisible: one produces a touch nobody can defend, the other loses an asset the company paid for.
- **Every touch resets the clock, whoever made it.** A clock that only counts EMBER's own touches is not tracking what the contact experienced.
- **Consent and permanent exit state are checked before composition, not after.** A composed message on an exited channel is a message that gets sent by accident.
- **The handoff from FORGE is accepted explicitly.** An assumed handoff produces a file owned by neither agent, and nobody notices for years.
- **Any reason involving a rate, payment, term, or cost figure routes through [`figure-disclosure-route`](../figure-disclosure-route/SKILL.md)** — the reason is where the figure most often first appears.

## Measured on

Dormant pool touched within SLA · touches made without a stated reason (target zero) · contacts with no next-touch date (target zero) · unsubscribe rate under 0.3%
