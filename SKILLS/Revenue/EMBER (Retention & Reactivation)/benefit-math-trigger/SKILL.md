---
name: benefit-math-trigger
description: Watches market movement against a known position, value changes, life events, anniversaries, and eligibility windows, and fires only when the math genuinely benefits the contact — with the math attached. Fires on every qualifying condition change.
agent: EMBER
division: Revenue
binding: mandate
---

# Benefit-Math Trigger

The reason to call is that the numbers changed in the contact's favor, and the numbers come with the call.

## When this fires

- On a qualifying market change from VANTAGE's `ember-trigger-feed`.
- On a value change, anniversary, or eligibility window opening on a contact's own record.
- On a life event the contact themselves disclosed.
- Never while VANTAGE reports its feed stale.

## Inputs

- The contact's position as recorded at close, with its estimate labels intact.
- Market conditions from VANTAGE's `ember-trigger-feed`, with source, as-of date, and staleness state.
- The account's configured benefit thresholds — what size of change is worth a conversation.
- The contact's own stated timeline and priorities from the handoff record.

## Procedure

1. **Read VANTAGE's staleness state first.** A stale feed produces no triggers, and the absence is reported as staleness rather than as an absence of market movement.
2. **Compute the benefit against the contact's recorded position**, carrying every estimate label through.
3. **Test the benefit against the configured threshold**, and fire only where it clears — a marginal benefit is a reason to say nothing.
4. **Test it against the contact's own stated priorities**, not only the arithmetic.
5. **Attach the math to the trigger**: the position assumed, the change observed, the resulting benefit, the assumptions, and the as-of dates.
6. **Route any resulting message through [`figure-disclosure-route`](../figure-disclosure-route/SKILL.md)** before it goes anywhere.
7. **Record fired and withheld triggers alike**, so the threshold can be evaluated against outcomes.

## Output

- A trigger carrying the full math — position, change, benefit, assumptions, and as-of dates.
- A withheld trigger where the benefit did not clear the threshold or the feed was stale.
- A record of both, for threshold calibration.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **The math is attached to the trigger and travels with any message that comes from it — this is EMBER's own boundary.** A benefit claim without its math is a sales pitch, and the contact cannot check it.
- **A stale feed produces no triggers, and the staleness is stated.** VANTAGE withholds rather than publishing stale conditions, and an absence of triggers read as an absence of market movement is the same information with the opposite meaning.
- **Every figure keeps its estimate label.** The position came from SCOUT's property-derived estimate and FORGE's recorded terms, and a benefit computed from a labeled estimate is itself an estimate.
- **A marginal benefit is not a reason to make contact.** Firing on a change that barely clears the arithmetic burns the trigger's credibility, and the next one — the one that genuinely matters — arrives to someone who has stopped reading.
- **The trigger never states or implies that the contact qualifies for anything.** A benefit that exists in the math is not an eligibility outcome, and the distinction disappears fast in a message about savings.
- **No trigger fires on a life event the company inferred rather than the contact disclosed.** Inferring a birth, a divorce, a death, or an illness from behavioral data and calling about it is the single most invasive thing this agent could do.
- **Withheld triggers are recorded too.** A threshold can only be calibrated against what it declined, and a log of only the sends makes it look perfect.

## Measured on

Reactivation to opportunity · triggers fired without attached math (target zero) · triggers fired on a stale feed (target zero) · trigger-to-conversation rate against the threshold
