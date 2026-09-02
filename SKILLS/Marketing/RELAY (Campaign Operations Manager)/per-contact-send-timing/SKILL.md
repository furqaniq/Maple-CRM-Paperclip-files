---
name: per-contact-send-timing
description: Schedules against each contact's own optimal window instead of one blast hour, inside AEGIS's quiet-hours floor. Fires at scheduling, on a shift in a contact's engagement pattern, and again at send time to re-verify the window still holds.
agent: RELAY
division: Marketing
binding: mandate
---

# Per-Contact Send Timing

Quiet hours are a floor RELAY narrows, never a preference RELAY optimizes against.

## When this fires

- At scheduling, for every send.
- When a contact's engagement pattern shifts enough to move their window.
- At send time, to re-verify the window is still valid before release.
- After any outage or backlog, before the queue drains.

## Inputs

- Per-contact engagement history by hour and day.
- The contact's timezone, from the record.
- **AEGIS's quiet-hours window** for the contact's jurisdiction and channel.
- Frequency suppression state for the window.
- The campaign's own window and any deadline attached to it.
- Carrier throughput limits, which bound how fast a window can be filled.

## Procedure

1. **Derive the contact's optimal window from their own engagement**, not from an account-wide average and not from a blast hour.
2. **Intersect it with AEGIS's quiet-hours window** for that contact's jurisdiction and channel. The quiet-hours window is the outer bound; the optimal window can only narrow it.
3. **Establish the timezone from the contact record**, not from the area code — a mobile number's area code says where the number was issued, not where the person is.
4. **Where the timezone cannot be established, use the most restrictive quiet-hours window that could apply**, never the account's own and never the widest.
5. **Check frequency suppression** for the window before committing the slot.
6. **Re-verify at send time.** A send that has slipped outside its window is rescheduled to the next valid one — it is not released late.
7. **After a backlog or outage, reschedule rather than drain.** The queue does not empty into whatever hours are available.

## Output

A per-contact send slot inside the intersection of the contact's engagement window and the applicable quiet-hours window, with the timezone basis recorded — and a reschedule, never a late release, for anything that missed it.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from RELAY — these apply to every RELAY skill, per `AGENTS.md` §5:**

- A/B and multivariate winners are declared only on a **real statistical threshold**. An inconclusive test is reported as inconclusive; noise is never dressed as a result.
- **Cross-campaign suppression is enforced** — no contact receives multiple unrelated sends in a day, no matter which campaign or which agent owns each one.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.

**Specific to this skill:**

- **Quiet hours are a hard floor RELAY schedules inside, never a preference RELAY optimizes against.** An engagement-optimal 9pm is not sent at 9pm. AEGIS owns the window and RELAY owns only the choice of moment within it.
- **An unknown or unverifiable timezone resolves to the most restrictive applicable window** — never to the account's own timezone, and never to the widest window that might apply.
- **A send that misses its window is rescheduled, not released late.** A backlog is never drained by sending outside quiet hours, and a deadline on the campaign never converts into permission to send at a prohibited hour.
- **Timing is never a route around suppression or consent.** A better hour is not a path to a contact who is suppressed or has opted out; the window governs *when*, never *whether*.
- **A single account-wide send hour is a failure of this skill**, not a simplification of it — the per-contact window is the whole capability.

## Measured on

Cost per engaged contact · delivery rate · complaint and bounce rates
