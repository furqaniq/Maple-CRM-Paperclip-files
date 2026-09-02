---
name: enrichment-fetcher
description: Attaches property characteristics, ownership tenure, position estimates, market context, source lineage, and prior interaction history to a resolved record. Runs in parallel with the intake event rather than gating it.
agent: SCOUT
division: Revenue
binding: mandate
---

# Enrichment Fetcher

The record a producer opens should already answer the questions they were going to spend the first call asking.

## When this fires

- Immediately on every resolved record, in parallel with [`instant-intake-event`](../instant-intake-event/SKILL.md).
- On a re-enrichment cycle for records whose attached data has aged past its freshness window.
- When a correction to the record invalidates a previously fetched attribute.

## Inputs

- The resolved record and its identifiers.
- Property and public-record data for the subject address.
- Market context from VANTAGE's `geo-granular-market-feed`, with its source and as-of date.
- Prior interaction history across every channel, and the per-contact brief from ATLAS's `memory-brief-store`.

## Procedure

1. **Fetch property characteristics and ownership tenure** from property and public-record sources.
2. **Derive any position figure from property data only**, and attach the derivation and its inputs to the figure.
3. **Label every position figure as a property-derived estimate**, inseparably from the figure itself, so it survives being copied into a message, a screen, or another agent's input.
4. **Attach market context from VANTAGE with its source and as-of date carried through**, never restated as a bare number.
5. **Attach prior interaction history**, including exits and complaints, not only the favorable history.
6. **Mark each attribute as fetched, pending, or unavailable** — never leave a pending attribute indistinguishable from a confirmed one.
7. **Write the completed enrichment to the record and update the per-contact brief.**

## Output

- The enriched record with each attribute marked fetched, pending, or unavailable.
- Position figures carrying their estimate label and their property-data derivation.
- Market context carrying VANTAGE's source and as-of date.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SCOUT — these apply to every SCOUT skill, per `AGENTS.md` §5:**

- SCOUT **never pulls, requests, infers, or stores consumer credit information**, under any circumstance.
- Position estimates are **always labeled as estimates** — never presented as fact.

**Specific to this skill:**

- **Every position figure is labeled a property-derived estimate, and the label travels with the figure.** A number that loses its label one hop downstream is read as a fact about someone's equity by the agent, the producer, and eventually the customer.
- **No position figure is derived from, checked against, or reconciled with consumer credit information** — see [`credit-data-firewall`](../credit-data-firewall/SKILL.md). Property-derived means property data only, and there is no supplementary source that improves it.
- **Pending is a distinct state from unavailable and from zero.** An enrichment still in flight that reads as an empty field tells VOX and ECHO the company knows nothing, when in fact it does not yet know.
- **Enrichment never blocks the intake event.** The speed-to-lead window is measured in seconds and a fetch is not, which is exactly why these run in parallel.
- **Unfavorable history is attached with the rest.** Prior complaints, prior exits, and prior disputes are part of what an agent needs before it opens its mouth, and an enrichment that carries only the useful history is a briefing that sets someone up to say the wrong thing.
- **Market figures keep VANTAGE's as-of date.** A stale market number stripped of its date is indistinguishable from a current one, and EMBER's benefit math and PULSE's scenarios both read this field.

## Measured on

Enrichment coverage · position figures shipped without an estimate label (target zero) · intake events delayed by enrichment (target zero) · attribute freshness
