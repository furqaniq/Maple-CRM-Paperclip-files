---
name: no-show-recovery
description: Re-engages a missed appointment within minutes rather than the next day, while recovery odds still exist. Fires the moment an appointment passes unattended.
agent: TEMPO
division: Revenue
binding: mandate
---

# No-Show Recovery

Recovery odds on a no-show collapse within the hour, which is why this is a minutes rule and not a daily sweep.

## When this fires

- The moment an appointment passes its start without the contact attending.
- On a contact cancelling inside the window, which is treated as a no-show for recovery purposes.
- Never on an appointment the company itself missed — that escalates to a human instead.

## Inputs

- The missed appointment, its type, and its value to the pipeline.
- The contact's channel consent, permanent exit state, and permissible contact window.
- The producer's live availability for an immediate re-offer, from [`availability-engine`](../availability-engine/SKILL.md).
- Prior no-show history on this contact.

## Procedure

1. **Fire within minutes of the missed start**, not on a sweep and not the next morning.
2. **Reach out on the channel the contact has been using**, with a re-offer of two real held slots rather than an open invitation.
3. **Treat this as a transactional response to a booking the contact made**, not as a re-engagement of a dormant contact.
4. **Hold to the permissible contact window** — an appointment missed at 8pm is recovered when the window opens, and the recovery is still the first thing that happens.
5. **Escalate to the producer rather than re-attempt after the second miss**, and never chase a third time.
6. **Escalate immediately where the company missed the appointment**, and never send the contact a recovery message for the company's own failure.
7. **Record the miss, the recovery attempt, and the outcome**, and feed repeat no-show patterns to LEDGER rather than acting on them here.

## Output

- A recovery contact within minutes, carrying two real held slots.
- A producer escalation after a second miss, rather than a third attempt.
- A human escalation where the company was the party that missed.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TEMPO — these apply to every TEMPO skill, per `AGENTS.md` §5:**

- No-show recovery attempts happen **within minutes**, never deferred to the next day.
- Every auto-generated task carries a **real owner, a real due date, and context** — never a bare reminder with no accountable party.
- Tasks blocking money are **escalated**, not left in the standard queue to age out.

**Specific to this skill:**

- **A no-show recovery is transactional and is not governed by EMBER's twenty-one-day collision guard.** The guard covers dormant-pool outreach — a contact nobody has a live reason to call. Someone who booked an appointment this morning and missed it at two is in flight, not dormant, and a twenty-one-day silence rule applied to them means the recovery never happens at all, which is the same as not having this skill. EMBER's `21-day-collision-guard` states the same carve-out from its side.
- **Consent and permanent exit still apply in full.** Transactional is a carve-out from frequency, never from consent, and a contact who exited a channel is not recovered on it.
- **Two held slots, never an open invitation.** The same rule as ECHO's closer, for the same reason, at the moment it matters most.
- **Two attempts, then a human.** A third automated chase on a missed appointment is where recovery becomes pressure, and the contact's read of the company changes permanently.
- **The company's own miss is never recovered by messaging the customer.** Asking someone to rebook an appointment the company failed to attend, automatically, within minutes, is the worst possible version of being responsive.
- **Recovery holds to the permissible contact window.** Within minutes means at the first permitted minute, not at any minute.
- **Repeat no-show patterns go to LEDGER, not into a local rule.** Down-prioritizing someone for missing twice is a routing decision made without review, and it belongs where it can be seen.

## Measured on

No-show recovery rate · median time from miss to recovery attempt · third automated attempts (target zero) · recovery messages sent for the company's own misses (target zero)
