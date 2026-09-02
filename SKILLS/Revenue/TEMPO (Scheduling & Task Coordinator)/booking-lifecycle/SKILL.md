---
name: booking-lifecycle
description: Books, confirms, reminds, and reschedules without a human touching a calendar, treating appointment communications as transactional rather than marketing. Fires on every booking request and at every lifecycle point after it.
agent: TEMPO
division: Revenue
binding: mandate
---

# Booking Lifecycle

The whole lifecycle — book, confirm, remind, reschedule — belongs to one owner, or the reminder goes out for an appointment that moved.

## When this fires

- On a booking request from ECHO's `two-slot-closer`, VOX, a producer, or a contact.
- At each configured confirmation and reminder point before an appointment.
- On any reschedule or cancellation, from either side.
- On a booking whose owner has left or whose seat was revoked.

## Inputs

- The requested slot and its hold, from [`availability-engine`](../availability-engine/SKILL.md).
- The contact's timezone, channel consent, and permanent exit state.
- The appointment type, its duration, and its location or connection details.
- The reminder schedule configured for this appointment type.

## Procedure

1. **Convert the hold into a booking atomically**, so a slot cannot be lost between the hold and the write.
2. **Confirm to every party in their own timezone**, with what the appointment is, where, and with whom.
3. **Send reminders on the configured schedule**, as transactional messages tied to a booking the contact made.
4. **Re-verify the appointment still stands immediately before each reminder**, so a reminder is never sent for something that moved an hour ago.
5. **Handle a reschedule as one operation** — release the old slot, hold and book the new one, and reissue confirmations — rather than as a cancel and a separate booking.
6. **Release the slot immediately on cancellation**, and hand a no-show to [`no-show-recovery`](../no-show-recovery/SKILL.md).
7. **Reassign a booking whose owner has left** to a named human rather than cancelling it on the customer.

## Output

- A confirmed booking with confirmations issued to every party in their own timezone.
- Reminders sent on schedule, each re-verified against the live booking.
- A reschedule handled as a single operation, with the old slot released.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TEMPO — these apply to every TEMPO skill, per `AGENTS.md` §5:**

- No-show recovery attempts happen **within minutes**, never deferred to the next day.
- Every auto-generated task carries a **real owner, a real due date, and context** — never a bare reminder with no accountable party.
- Tasks blocking money are **escalated**, not left in the standard queue to age out.

**Specific to this skill:**

- **An appointment confirmation, reminder, or reschedule notice is a transactional message and is never suppressed by a marketing frequency cap.** RELAY's `cross-campaign-suppression` holds the cross-agent count and names transactional sends as the ones that never yield: a newsletter is not a reason a customer misses the appointment they booked. It still obeys consent and the contact's permanent exit state — those are boundaries, not frequency.
- **Reminders re-verify against the live booking immediately before sending.** A reminder for an appointment that was moved an hour ago is worse than no reminder, because the customer acts on it.
- **A reschedule is one operation.** Cancel-then-book loses the slot to someone else in between and confirms a cancellation the customer never asked for.
- **Every party gets the time in their own timezone.** This is where multi-party bookings actually fail.
- **A booking whose owner has left is reassigned, never cancelled on the customer.** WARDEN's same-day revocation is correct and the customer's appointment is not the thing that should absorb it.
- **A contact under a permanent exit receives no reminder on the exited channel**, and the booking is confirmed through a channel that remains open or through the producer directly.
- **No appointment is booked to imply an outcome.** An invitation to "come in and sign" on a file that is not approved is an eligibility statement in the calendar.

## Measured on

Booking conversion · reminders sent for moved or cancelled appointments (target zero) · appointments cancelled because their owner left (target zero) · reschedules completed as one operation
