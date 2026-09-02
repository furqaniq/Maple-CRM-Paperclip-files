---
name: two-slot-closer
description: Converts conversation to a booked appointment by offering two real slots held against live availability — never an open-ended "when works for you." Fires when the conversation reaches a point where a meeting is the next step.
agent: ECHO
division: Revenue
binding: mandate
---

# Two-Slot Closer

Two specific times get answered; an open invitation gets deferred, and the deferral is where the appointment dies.

## When this fires

- When the conversation reaches a point where a meeting is the natural next step.
- When a contact asks to meet, speak, or be called.
- On a re-offer where a held pair expired unanswered.

## Inputs

- Live availability from TEMPO's `availability-engine`, for the assigned producer.
- The contact's timezone and any stated preference.
- The assignment from SCOUT's `capacity-aware-router`.
- The hold state on any slots already offered to other contacts.

## Procedure

1. **Read live availability from TEMPO**, never from a cached window and never from ECHO's own copy.
2. **Place a hold on both slots through TEMPO before offering them**, so the same slot is never offered to two contacts at once.
3. **Offer exactly two real slots**, in the contact's timezone, stated as specific times rather than as a range.
4. **Book through TEMPO's `booking-lifecycle` on acceptance**, and release the unused hold immediately.
5. **Release both holds on a decline, a non-answer past the hold window, or an exit**, rather than letting them sit against the calendar.
6. **Offer a second pair once where the first was declined on timing**, and hand to a human rather than offering a third.
7. **Never offer an open-ended alternative** — the fallback for two declined pairs is a human, not "let me know what works."

## Output

- Two specific held slots offered in the contact's timezone.
- A booking through TEMPO on acceptance, with the unused hold released.
- A released hold pair and a human handoff where the offer did not convert.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ECHO — these apply to every ECHO skill, per `AGENTS.md` §5:**

- ECHO **never improvises objection handling** — only from the compliance-approved library.
- ECHO **exits permanently** on opt-out, hostility, legal language, or wrong number — no further contact, no exceptions.
- Complaints and distress are escalated to a human **within the same minute**, never batched.

**Specific to this skill:**

- **Both slots are held before they are offered, and released the moment they are not needed.** Offering unheld availability means two contacts accept the same time and one of them gets a cancellation from a company that just told them it was free — and offering held slots that are never released is how a producer's calendar fills up with appointments nobody booked.
- **Availability comes from TEMPO and bookings go through TEMPO.** ECHO offers and confirms; it never writes to a calendar directly, or the calendar has two owners and one of them is wrong.
- **No open-ended invitation, ever.** "When works for you" is the phrasing this skill exists to eliminate, and it reappears most often as a polite fallback after a decline.
- **A slot is offered in the contact's timezone.** An offer in the office's timezone is a booking error the contact has to catch, and half of them will not.
- **Two declined pairs go to a human, not a third pair.** A third round is where a booking attempt becomes pressure, and pressure is what HONE's compliance patterns are watching for.
- **No booking is attempted on a contact under a permanent exit**, whatever stage the conversation had reached when the exit fired.
- **A booking is never used to imply an outcome.** "Let's get you booked in to finalize your approval" is an eligibility statement with a calendar invite attached.

## Measured on

Appointment set rate · double-booked offers (target zero) · holds left unreleased (target zero) · open-ended invitations sent (target zero)
