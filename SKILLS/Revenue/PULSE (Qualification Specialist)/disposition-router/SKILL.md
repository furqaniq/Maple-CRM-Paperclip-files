---
name: disposition-router
description: Assigns fast-track, nurture, park-with-a-named-reactivation-trigger, or disqualify-with-reason as an internal routing state. Fires on every completed scoring pass and on any event that changes a standing disposition.
agent: PULSE
division: Revenue
binding: mandate
---

# Disposition Router

Four internal states, each with a named reason and a named next step — and none of them is a message to the customer.

## When this fires

- On every completed scoring pass.
- On any event that would change a standing disposition — a reply, a market move, an expired reason.
- When a parked contact's named reactivation trigger fires.

## Inputs

- The separate intent and fit scores, with coverage and contributing factors.
- Discovery answers and any complexity flag.
- The reason a prior disposition was set, where one exists.
- EMBER's next-touch state, so a park hands over to a real clock rather than a label.

## Procedure

1. **Assign exactly one of the four states**, with a written reason naming the specific evidence.
2. **Attach a named reactivation trigger to every park**, never a bare date — the trigger is what makes a park different from an abandonment.
3. **Hand a park and its trigger to EMBER's `next-touch-clock`**, so the clock lives with the agent that owns dormant contacts.
4. **Fast-track to the assigned producer** with both scores, coverage, and contributing factors attached.
5. **Record a disqualification as an internal routing state**, with its reason and the condition under which it would expire.
6. **Keep every disposition reversible**, and re-evaluate on the events that would change it.
7. **Emit nothing to the customer from this skill.** Any communication that follows is ECHO's, EMBER's, or a producer's, under their own rules.

## Output

- One internal disposition with a written reason and, for a park, a named reactivation trigger.
- A fast-track hand-off carrying both scores and their coverage.
- A disqualification record with its reason and its expiry condition — internal only.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from PULSE — these apply to every PULSE skill, per `AGENTS.md` §5:**

- PULSE **never states or implies an approval, denial, pre-approval, or eligibility outcome** — to a customer, in any channel, at any confidence level.
- A disqualification is an **internal routing state only** — never communicated to a customer as a decision.
- Autonomy is **permanently capped at L2** — this cap cannot be raised by AEGIS promotion or any other mechanism.

**Specific to this skill:**

- **A disqualification is an internal routing state and never reaches a customer as a decision.** Not as a message, not as a status a customer portal displays, not as a stopped sequence someone explains, not as a producer's paraphrase of what the system said. The state exists to route work inside the company; the moment it is communicated it is a denial, and PULSE does not issue denials — see [`outcome-language-interlock`](../outcome-language-interlock/SKILL.md).
- **Every disposition carries a written reason naming the evidence.** A state with no reason cannot be reviewed for bias, cannot be reversed by a human who disagrees, and cannot be explained to a regulator.
- **Every park carries a named trigger, and the trigger is handed to EMBER.** A park with a date and no reason is a contact nobody will ever have a reason to call, which is a deletion with extra steps.
- **Every disqualification carries the condition under which it expires.** Circumstances change, and a permanent internal state on a contact whose situation resolved is a lead the company already paid for and then threw away.
- **Dispositions are reversible and re-evaluated.** A state set on eight-week-old evidence is a decision about a person who no longer exists.
- **Disposition never rests on a protected characteristic or a proxy for one**, and a disposition pattern that correlates with one is escalated to AEGIS rather than explained away. Routing that systematically parks a neighborhood is redlining regardless of what each individual reason said.
- **PULSE is capped at L2 here.** This skill sets an internal state and hands off; it never sends, never closes a file, and never acts on its own disposition.

## Measured on

Fast-track override rate · disqualifications communicated to a customer (target zero, and any non-zero value is an incident) · parks carrying a named trigger · dispositions reversed on re-evaluation
