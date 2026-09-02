---
name: scenario-calculator
description: Produces indicative figures for internal discussion, clearly labeled as estimates with their assumptions attached. Fires when a scenario would inform routing, prioritization, or a producer's preparation.
agent: PULSE
division: Revenue
binding: mandate
---

# Scenario Calculator

An indicative figure is a thinking tool for the team, and the labeling is what stops it becoming a promise to the customer.

## When this fires

- When a scenario figure would inform routing, prioritization, or a producer's call preparation.
- On a material change to the inputs a prior scenario rested on.
- On a request for an indicative figure from a producer or another agent.

## Inputs

- Discovery answers relevant to the scenario.
- Property-derived position estimates from SCOUT, with their labels intact.
- Market context from VANTAGE's `geo-granular-market-feed`, with source and as-of date.
- The assumption set the scenario requires, and which assumptions have no basis in the record.

## Procedure

1. **Enumerate the assumptions the scenario needs** before computing anything, and mark each as sourced from the record or supplied as a placeholder.
2. **Refuse to compute where the load-bearing assumptions are all placeholders.** A figure resting on nothing is not an estimate, it is a number.
3. **Compute the scenario**, carrying the estimate labels on every input figure through to the output.
4. **Attach the assumptions to the figure inseparably**, so the figure cannot be copied, screenshotted, or forwarded without them.
5. **Label the output as an indicative estimate for internal discussion**, on the figure itself rather than in a footer.
6. **Mark the market inputs with VANTAGE's as-of date**, so a scenario built on a three-week-old rate is visibly that.
7. **Route to AEGIS's `disclosure-builder` and `eligibility-language-blocker` before any version of the figure goes to a customer**, and never send one directly.

## Output

- An indicative figure with its full assumption set attached and its estimate label on the figure.
- A refusal to compute where the load-bearing assumptions had no basis, naming what is missing.
- A gated hand-off where a figure is intended for a customer.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from PULSE — these apply to every PULSE skill, per `AGENTS.md` §5:**

- PULSE **never states or implies an approval, denial, pre-approval, or eligibility outcome** — to a customer, in any channel, at any confidence level.
- A disqualification is an **internal routing state only** — never communicated to a customer as a decision.
- Autonomy is **permanently capped at L2** — this cap cannot be raised by AEGIS promotion or any other mechanism.

**Specific to this skill:**

- **Every figure is labeled an estimate, and the label attaches to the figure rather than to the document.** A figure survives a screenshot, a paste, and a forward; a footer does not, and the figure is what arrives.
- **The assumptions travel with the figure, always.** A payment estimate stripped of its rate, term, and position assumptions is a quote, and it is a quote the company did not intend to give.
- **A scenario is never presented, framed, or worded as an approval, a pre-approval, an eligibility outcome, or what someone qualifies for** — see [`outcome-language-interlock`](../outcome-language-interlock/SKILL.md). This holds however the request was worded and however clearly the requester says they understand it is an estimate.
- **Nothing goes to a customer without AEGIS.** Any figure reaching a contact routes through `disclosure-builder` and `eligibility-language-blocker` first, and this skill has no direct outbound path at all.
- **No credit input, and no substitute constructed to stand in for one.** A scenario is exactly where an inferred score would be most useful and most damaging.
- **Stale market inputs are stated as stale, not silently used.** A scenario computed on an expired rate is wrong in the direction the customer will notice.
- **A scenario is never re-computed to reach a preferred number.** Adjusting assumptions until the figure looks good produces a figure that is true of no one.

## Measured on

Scenario figures shipped without assumptions attached (target zero) · figures reaching a customer without the AEGIS gate (target zero) · scenarios refused for placeholder-only assumptions
