---
name: multi-party-coordinator
description: Converges every outside participant onto one time without the email chain, exposing nothing about the file to any party beyond what they need to attend. Fires when an appointment needs more than one outside participant.
agent: TEMPO
division: Revenue
binding: mandate
---

# Multi-Party Coordinator

The email chain is where multi-party scheduling dies; the trap is that converging on a time quietly exposes who else is involved.

## When this fires

- When an appointment requires more than one outside participant.
- When a converged time falls through and the set has to reconverge.
- When a participant is added to or removed from an in-progress coordination.

## Inputs

- The participant set — internal and external — and what each is being invited to.
- Internal availability from [`availability-engine`](../availability-engine/SKILL.md), with holds.
- Each external party's contact channel, consent, and timezone.
- What each participant is permitted to know about the file and about the other participants.

## Procedure

1. **Determine what each party may see** before sending anything — participants, purpose, and file details are separate disclosures.
2. **Hold the candidate internal slots** so convergence is not racing against ordinary bookings.
3. **Offer candidate times to each external party in their own timezone**, individually rather than on a shared thread.
4. **Converge on the first mutually available time** and book it, releasing every other hold immediately.
5. **Chase non-responders on their own cadence**, with an attempt limit, and escalate to a human rather than chasing indefinitely.
6. **Reconverge as one operation** where a party drops out, rather than cancelling and restarting.
7. **Confirm to every party in their own timezone**, with only the details that party is permitted to have.

## Output

- A single booked time with every party confirmed in their own timezone.
- Per-party disclosure limited to what that party may know.
- A human escalation where convergence failed inside the attempt limit.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TEMPO — these apply to every TEMPO skill, per `AGENTS.md` §5:**

- No-show recovery attempts happen **within minutes**, never deferred to the next day.
- Every auto-generated task carries a **real owner, a real due date, and context** — never a bare reminder with no accountable party.
- Tasks blocking money are **escalated**, not left in the standard queue to age out.

**Specific to this skill:**

- **No party learns who else is involved unless they are entitled to.** A shared invitation, a visible recipient list, or a calendar entry naming every attendee discloses the parties to each other — and on a file with an agent, an attorney, a partner, and a spouse who is not on the loan, that disclosure is the mistake, not the scheduling.
- **External parties are contacted individually, not on a common thread.** A reply-all on a coordination thread is an uncontrolled disclosure with the company's name at the top of it.
- **Consent, permissible contact windows, and exit state apply to external participants exactly as to contacts.** A partner or an outside agent is a person the company is messaging.
- **Every offer and every confirmation is in that party's own timezone.** Multi-party is where timezone errors compound rather than cancel.
- **Holds are released the moment convergence lands.** Unreleased holds across a multi-party attempt take out a producer's whole week.
- **A failed convergence escalates to a human inside the attempt limit.** Chasing four external parties indefinitely is how the coordination becomes the delay.
- **No file detail travels in a scheduling message beyond the purpose the party needs.** A calendar invitation is the least-guarded document the company sends and it reaches the most parties.

## Measured on

Time to schedule multi-party meetings · convergence rate inside the attempt limit · participant details disclosed to a party not entitled to them (target zero) · holds released on convergence
