---
name: closed-lost-recycler
description: Re-engages a lost lead only once the reason it was lost has actually expired, rather than on a calendar. Fires when a recorded loss reason expires or is superseded.
agent: EMBER
division: Revenue
binding: mandate
---

# Closed-Lost Recycler

A lost lead is worth re-engaging exactly when the thing that lost it stopped being true, and not a day before.

## When this fires

- When a recorded loss reason expires — a timeline passed, a condition resolved, a market moved.
- When a market change from VANTAGE supersedes the reason a lead was lost.
- Never on a schedule and never on a lead whose reason still holds.

## Inputs

- The loss reason as recorded, with its expiry condition.
- PULSE's disposition record, where the loss was a disqualification with a stated expiry condition.
- Market conditions from VANTAGE's `ember-trigger-feed`, with staleness state.
- Channel consent, permanent exit state, and prior recycling history.

## Procedure

1. **Read the recorded loss reason and its expiry condition.** A loss with no recorded reason is surfaced rather than recycled.
2. **Test whether the condition has genuinely expired**, against the record and the market rather than against elapsed time.
3. **Never recycle a lead lost to an exit, a complaint, or a hostility signal.** Those reasons do not expire.
4. **Compose the re-engagement around what changed**, so the contact is told why they are hearing from the company now.
5. **Say nothing about the internal disposition.** A disqualification is an internal routing state and never surfaces to the contact in any form.
6. **Apply the twenty-one-day window and consent state**, and route any figure through [`figure-disclosure-route`](../figure-disclosure-route/SKILL.md).
7. **Recycle once per expired reason**, then return the contact to the ordinary next-touch clock.

## Output

- A re-engagement built on the specific change that expired the loss reason.
- A surfaced lead where the loss reason was never recorded.
- A permanent exclusion where the loss was an exit, a complaint, or hostility.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **A lead lost to an opt-out, hostility, legal language, a wrong number, or a complaint is never recycled.** Those are not conditions that expire, and a recycler that treats an exit as a stale disposition is the mechanism by which a permanent exit gets undone twelve months later, quietly, with a plausible business reason attached.
- **The reason must have genuinely expired, not merely aged.** Elapsed time is not a change in circumstances, and "it has been a year" is the calendar-driven recycling this skill exists to replace.
- **PULSE's disqualification never reaches the contact, in any form.** Not as an explanation of why they are being contacted again, not as a reference to a previous decision, not as an apology for one. It is an internal routing state under PULSE's boundary and it stays internal here.
- **The re-engagement names what changed.** A contact who was told no eighteen months ago and hears nothing about what is different will read the message as the company having forgotten.
- **A loss with no recorded reason is surfaced, not recycled on assumption.** Guessing why a lead was lost produces a re-engagement built on a fiction.
- **One recycle per expired reason.** Repeatedly re-engaging on the same expiry is a cadence with a justification attached.
- **Stale market conditions produce no recycling.** A recycle triggered by a market change VANTAGE has withdrawn is a message about something that did not happen.

## Measured on

Reactivation to opportunity from recycled leads · exited or complained contacts recycled (target zero, and any non-zero value is an incident) · recycles on unexpired reasons (target zero) · unsubscribe rate under 0.3%
