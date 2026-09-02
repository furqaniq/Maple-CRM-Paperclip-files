---
name: anomaly-detector
description: Surfaces a dead source, collapsing contact rate, stalled stage, or two-sigma move on any person within twenty-four hours, with evidence separated from hypothesis. Runs continuously against every monitored series.
agent: LEDGER
division: Operations
binding: mandate
---

# Anomaly Detector

Twenty-four hours is the promise. Evidence separated from hypothesis is what makes the alert worth acting on.

## When this fires

- Continuously, against every monitored series — source, campaign, stage, channel, and person.
- The moment a source goes dead, a contact rate collapses, a stage stalls, or a person moves two sigma.
- Within twenty-four hours of the change appearing in the data, without exception.

## Inputs

- Live series for every monitored dimension.
- Established baselines and their normal variance.
- Cohort maturity context from [`vintage-cohort-analyzer`](../vintage-cohort-analyzer/SKILL.md), so a young cohort is not read as a collapse.
- Known operational changes — a campaign ending, a person on leave, a suppression event — that explain a move without it being an anomaly.

## Procedure

1. **Compare each series against its own baseline and normal variance.**
2. **Check the cohort context before calling a collapse.** A conversion rate that looks like it fell may be a cohort that has not aged.
3. **Check for a known operational cause** — a paused campaign, a leave, a deliberate suppression — and report the move with its cause attached rather than as a mystery.
4. **Surface within twenty-four hours**, always, including when the cause is not yet understood.
5. **State the evidence first and the hypothesis second, visibly separated and visibly labeled.**
6. **Route to whoever can act**, since LEDGER cannot act itself.
7. **Report a detection that turns out to be nothing as a closed detection**, rather than letting it disappear.

## Output

- An anomaly finding within twenty-four hours, naming the series, the move, and the baseline.
- Evidence and hypothesis as separate, labeled sections.
- A route to the agent or human who can act on it.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from LEDGER — these apply to every LEDGER skill, per `AGENTS.md` §5:**

- LEDGER is **advisory only, by design** — it never acts unilaterally on the numbers it produces, regardless of how confident the recommendation is.
- LEDGER **never softens an unfavorable number** to protect a prior decision or a stakeholder's investment in a channel.
- The daily brief stays **decision-first and under four hundred words** — evidence and hypothesis stay clearly separated, never blended.

**Specific to this skill:**

- **The twenty-four hour clock runs from the change appearing in the data, not from LEDGER understanding it.** An anomaly whose cause is unknown is surfaced on time and labeled unexplained — waiting for a clean explanation is how a dead source runs for a week.
- **Evidence and hypothesis are separated and labeled, never blended.** A hypothesis reported in the same voice as the evidence acquires the evidence's authority, and the next reader repeats it as a finding.
- **A cohort maturity effect is not an anomaly**, and calling one is how a working channel gets cut. The cohort check runs before the alert fires.
- **LEDGER surfaces; it never acts.** It does not pause a campaign, reassign a lead, adjust a budget, or change a routing rule on the strength of its own detection, however clear the detection is.
- **An anomaly on a person routes to `coaching-scorecard`'s constraints**, not straight to a manager as a raw two-sigma alert. A statistical move on an individual is not a performance verdict, and it must not arrive looking like one.
- **A detection that resolves to nothing is closed explicitly.** Detections that quietly disappear teach everyone to wait before acting on the next one.

## Measured on

Anomaly detection lag (target under twenty-four hours) · detection precision · anomalies surfaced before someone noticed manually
