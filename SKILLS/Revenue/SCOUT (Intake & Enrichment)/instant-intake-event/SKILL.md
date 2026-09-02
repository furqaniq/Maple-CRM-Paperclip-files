---
name: instant-intake-event
description: Fires the intake event immediately so first touch lands inside the speed-to-lead window, with enrichment continuing in parallel and provisional fields marked as such. Fires the moment identity is resolved.
agent: SCOUT
division: Revenue
binding: mandate
---

# Instant Intake Event

Speed to lead is the most predictive operational metric in this industry, so the event fires on the resolved record, not the finished one.

## When this fires

- The moment [`identity-resolver`](../identity-resolver/SKILL.md) produces a resolved record.
- On a re-fire when the first touch it triggered failed to complete.
- Never on a held-for-review pair, and never on a record the junk filter has rejected.

## Inputs

- The resolved record with whatever enrichment has landed so far.
- Per-attribute state — fetched, pending, or unavailable — from [`enrichment-fetcher`](../enrichment-fetcher/SKILL.md).
- Consent and exit state per channel, so the event never triggers a touch on a closed channel.
- The assignment from [`capacity-aware-router`](../capacity-aware-router/SKILL.md).

## Procedure

1. **Fire the event the moment identity resolves**, without waiting on enrichment, routing enrichment, or market context.
2. **Mark every pending attribute as pending in the event payload**, so the receiving agent knows the difference between a fact the company lacks and one that has not arrived yet.
3. **Carry consent and exit state in the event itself**, so no receiver has to make a second call to find out whether it may contact this person.
4. **Emit through ATLAS's `signal-router`** to the agents that own first touch — VOX for the call, ECHO for the written channels.
5. **Emit a follow-up event when enrichment completes**, so an agent that acted on a provisional record can be corrected rather than left with it.
6. **Re-fire once where the first touch did not complete**, and escalate rather than re-firing indefinitely.
7. **Suppress the event entirely for a junk rejection or a held-for-review pair**, and record the suppression.

## Output

- The intake event, carrying the resolved record, per-attribute state, consent and exit state, and the assignment.
- A completion event when enrichment lands, naming which attributes changed.
- A suppression record where the event was withheld and why.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SCOUT — these apply to every SCOUT skill, per `AGENTS.md` §5:**

- SCOUT **never pulls, requests, infers, or stores consumer credit information**, under any circumstance.
- Position estimates are **always labeled as estimates** — never presented as fact.

**Specific to this skill:**

- **Enrichment never gates the event.** The window is measured in seconds and this is the whole reason the two run in parallel.
- **Pending is stated in the payload, never rendered as empty.** An agent that reads a pending equity figure as an absent one will say the company has no information about a property it has fully enriched sixty seconds later — and an agent that reads a pending field as a *value* is worse.
- **No receiver states a provisional figure to a customer.** A pending attribute is context for the agent, and VOX and ECHO both operate on that basis; a figure still in flight is not a thing to say out loud.
- **The event carries consent and exit state.** A first touch is the highest-risk moment for contacting someone who already closed the channel, because it is the moment with the least deliberation in it.
- **A held-for-review pair fires nothing.** Triggering first touch on an unresolved identity means calling one of two people about the other one's file.
- **A failed first touch re-fires once and then escalates.** An event retrying on a loop is how a contact receives four calls in a minute, and ATLAS's `loop-breaker` should never have to be the thing that catches this.

## Measured on

Median time to first touch · events fired before consent state was attached (target zero) · provisional figures stated to a customer (target zero) · first-touch completion rate
