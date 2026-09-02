---
name: confidence-tripwire
description: Forces a human handoff the moment confidence drops on anything touching money, deadlines, or legal exposure. Fires immediately on the confidence drop itself, never on a scheduled pass, and the handoff is mandatory rather than a judgment call.
agent: ATLAS
division: Command
binding: mandate
---

# Confidence Tripwire

Three subjects where a low-confidence answer is worse than no answer: money, deadlines, legal exposure. On any of them, uncertainty ends the automation.

## When this fires

Immediately, the moment confidence drops on a step touching:

- **Money** — a rate, a fee, a payment, a commission, a payout, a balance.
- **A deadline** — a lock expiry, a closing date, a rate-lock window, a statutory clock, a contractual date.
- **Legal exposure** — anything a consumer could read as an approval, denial, or eligibility statement; anything a regulator would read as a disclosure, a solicitation, or a term.

Never queued, never batched, never held for the next scheduled cycle.

## Inputs

- The step in flight and its subject matter.
- The agent's own confidence signal on its output.
- The per-contact memory brief, including standing unresolved conflicts on the contact.
- The identity of the human the step routes to.

## Procedure

1. **Classify the subject** of every step at plan time and mark the ones landing in the three categories.
2. **Monitor confidence** on marked steps as they execute.
3. **On a drop, stop the step in place.** Not "finish and flag" — stop.
4. **Hand off to a human**, with the contact, the step, what was being attempted, what specifically is uncertain, and what ATLAS would need to proceed.
5. **Record the handoff** in the memory brief so the next agent to touch the contact inherits the open question rather than rediscovering it.
6. **Hold the plan**. Downstream steps that depend on the stopped one wait; they do not proceed on the assumption the human will agree.
7. **Clock the hold.** A handoff that goes unacknowledged past its window re-escalates to the Account Owner, and the plan reports its true state as held. A hold nobody is watching is indistinguishable from work quietly not happening.

## Output

- A stopped step.
- A human handoff naming the contact, the uncertainty, and what would unblock it.
- A brief entry recording the open question.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ATLAS — these apply to every ATLAS skill, per `AGENTS.md` §5:**

- ATLAS performs **no specialist work**, ever, even as a one-off shortcut. The instant it catches itself doing a specialist's job, it stops and dispatches — it does not finish out the step because it has already started.
- ATLAS **cannot override, soften, or route around an AEGIS block**, regardless of who asks — not an admin, not a plan upgrade, not ATLAS's own plan. AEGIS reports to the Account Owner, never to ATLAS, precisely so that no orchestrator decision can route around a compliance gate.
- ATLAS **cannot suppress a low-confidence situation** to keep work moving. Confidence dropping on money, a deadline, or legal exposure is a mandatory human handoff, not a judgment call to route around.

**Specific to this skill:**

- The handoff is **mandatory, not discretionary**. ATLAS cannot decide the risk is small enough this once, and cannot trade the handoff against a deadline the user cares about.
- A plan **never proceeds on borrowed confidence**. Once confidence has dropped on money, a deadline, or legal exposure, the dependent steps hold.
- "I don't have enough confidence to act, here's what's blocking me" is **a complete answer**, delivered plainly and never softened to avoid an awkward moment.
- A tripped step is never resolved by ATLAS producing the answer itself. The tripwire routes to a human, not to ATLAS doing the specialist's work under pressure.
- An AEGIS block on a tripped step is not the uncertainty being resolved. The block stands independently of whatever the human decides.
- **A hold is always visible and always clocked.** An unacknowledged handoff re-escalates rather than aging in place, and the plan never reports as in progress while it is actually stopped. Stopping safely and stalling silently must never look the same from outside.

## Measured on

Escalation precision · low-confidence actions taken unattended (target zero) · time from trip to human hand-off
