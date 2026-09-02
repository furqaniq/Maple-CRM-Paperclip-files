---
name: inbound-applicant-handler
description: Runs the screening conversation, scheduling, and packet delivery for inbound applicants, on production and licensure criteria only. Fires on every inbound application.
agent: HARBOR
division: People
binding: mandate
---

# Inbound Applicant Handler

An inbound applicant has already raised their hand, and the only thing that should decide what happens next is production and licensure.

## When this fires

- On every inbound application, from any channel.
- On an applicant responding to a screening question.
- On an application that cannot be progressed, which still receives an answer.

## Inputs

- The application and whatever the applicant supplied.
- The screening question set, cleared by [`protected-characteristic-interlock`](../protected-characteristic-interlock/SKILL.md).
- Licensing verification against public data.
- Availability from TEMPO's `availability-engine`, for scheduling.

## Procedure

1. **Acknowledge every application**, including the ones that will not progress.
2. **Ask only the screened questions**, on production, licensure, markets, and availability.
3. **Verify licence status against public data** rather than asking the applicant to assert it.
4. **Disregard unsolicited protected information the applicant volunteers**, and do not record it into any field.
5. **Schedule through TEMPO**, with the same care as a customer appointment.
6. **Deliver the information packet** from the approved, AEGIS-cleared set.
7. **Route every progression decision to a human**, with the production and licensure basis assembled.

## Output

- An acknowledged application and a completed screening on screened criteria only.
- A verified licence status from public data.
- A human-decision packet with the production and licensure basis assembled.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HARBOR — these apply to every HARBOR skill, per `AGENTS.md` §5:**

- HARBOR **never screens, ranks, or filters candidates on protected characteristics**, or on proxies for them — sourcing criteria are **production and licensure only**.
- **All candidate communications pass through AEGIS** with employment-communication rules applied, without exception.
- **Every hiring decision is human.** HARBOR builds the case; it never makes the call.

**Specific to this skill:**

- **Only screened questions are asked.** Age, family status, national origin, disability, religion, criminal history where it may not be asked, salary history where it may not be asked, and every conversational route to them are out of scope however naturally the exchange offers them.
- **Protected information an applicant volunteers is disregarded and not recorded.** Applicants routinely volunteer it; the firm's obligation is not to use it, and the reliable way to not use it is to not write it down.
- **No progression decision is made here.** HARBOR assembles the basis and a human decides — every hiring decision is human, and a screening handler is exactly where an automated rejection would feel most defensible and be least so.
- **Every application is acknowledged, including the ones that go nowhere.** Silence is the most common recruiting failure and the one candidates talk about publicly.
- **Every message passes through AEGIS with employment-communication rules applied.**
- **Licence status is verified against public data, not assumed from the applicant's claim** — and a lapsed licence is a fact to confirm with them, never a rejection issued automatically.
- **Packet content comes from the approved set.** An improvised description of compensation, culture, or opportunity is an unreviewed employment communication.

## Measured on

Applications acknowledged (must be 100%) · unscreened questions asked (target zero) · progression decisions made without a human (target zero) · time from application to first response
