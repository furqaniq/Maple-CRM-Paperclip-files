---
name: licensure-based-sourcer
description: Sources candidates from public licensing data, production records, and market movement, ranked by fit against what this firm actually needs. Fires on the sourcing cycle and on any change to the firm's stated need.
agent: HARBOR
division: People
binding: mandate
---

# Licensure-Based Sourcer

Production and licensure are the only criteria, and everything a résumé usually carries besides those two is a protected characteristic or a proxy for one.

## When this fires

- On the sourcing cycle.
- On a change to the firm's stated need — a market, a product line, a seat to fill.
- On a market movement signal from [`in-play-signal-monitor`](../in-play-signal-monitor/SKILL.md).

## Inputs

- Public licensing data — licence status, jurisdictions, standing, and tenure.
- Publicly available production records.
- The firm's stated need: markets, products, volume, and the seats it is filling.
- The screened criteria set, cleared by [`protected-characteristic-interlock`](../protected-characteristic-interlock/SKILL.md).

## Procedure

1. **Define the need in production and licensure terms** before sourcing anything — volume, product mix, markets licensed in, standing.
2. **Screen the criteria set through the interlock before it is used**, not after a list has been built from it.
3. **Source from public licensing and production data only.**
4. **Rank on fit against the stated need**, and state the ranking basis on every candidate.
5. **Discard fields that are not production or licensure** rather than collecting them and declining to rank on them.
6. **Test the resulting list for composition skew** against the licensed population, and escalate a skew rather than shipping the list.
7. **Hand the list to [`candidate-pipeline-governance`](../candidate-pipeline-governance/SKILL.md)** with its ranking basis attached.

## Output

- A ranked candidate list with the production and licensure basis stated per candidate.
- A screening record for the criteria set.
- A composition-skew escalation where the list diverges materially from the licensed population.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HARBOR — these apply to every HARBOR skill, per `AGENTS.md` §5:**

- HARBOR **never screens, ranks, or filters candidates on protected characteristics**, or on proxies for them — sourcing criteria are **production and licensure only**.
- **All candidate communications pass through AEGIS** with employment-communication rules applied, without exception.
- **Every hiring decision is human.** HARBOR builds the case; it never makes the call.

**Specific to this skill:**

- **Production and licensure only — this is HARBOR's own boundary.** Not school, not graduation year, not tenure gaps, not photographs, not name, not neighborhood, not the language of a public profile, not association memberships, not anything a résumé conventionally carries. Each of these is either a protected characteristic or a reliable proxy for one, and the whole sourcing model is that the two legitimate criteria are also the two that actually predict.
- **Fields that are not criteria are not collected.** Collecting a photograph and declining to rank on it still puts it in front of every human who reads the record, and the model that ranks on "fit" next quarter will find it.
- **The criteria set is screened before it is used, never after.** A list already built on an unscreened criterion has already made its selections, and screening the criterion afterward does not unmake them.
- **Composition skew against the licensed population is escalated, not explained.** A neutral criterion producing a skewed list is how disparate impact actually appears, and every individual criterion will look defensible when it does.
- **Public data only, and only data made public for professional purposes.** A licence record is public; a person's social presence, their family, and their personal life are not sourcing inputs however findable they are.
- **Ranking basis travels with every candidate.** A rank with no stated basis cannot be reviewed for bias and cannot be defended if it is challenged.
- **HARBOR sources; it never decides.** Every hiring decision is human, and a ranked list is an input to that decision rather than a shortlist that has already made it.

## Measured on

Candidates in pipeline · non-production, non-licensure criteria used (target zero) · criteria sets used before screening (target zero) · composition skew escalations raised
