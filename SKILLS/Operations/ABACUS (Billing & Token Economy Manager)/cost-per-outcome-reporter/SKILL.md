---
name: cost-per-outcome-reporter
description: Publishes cost per outcome beside every agent's performance so each digital employee's return is visible rather than assumed. Fires on the reporting cycle and whenever an agent's cost per outcome moves materially.
agent: ABACUS
division: Operations
binding: mandate
---

# Cost-Per-Outcome Reporter

Every agent is sold as an employee. This is the skill that publishes what each one costs against what it actually produced.

## When this fires

- On the scheduled reporting cycle.
- When an agent's cost per outcome moves materially against its baseline.
- When ATLAS assembles the cross-roster report.
- On demand, when an agent's value is being questioned.

## Inputs

- Consumption per agent from the meter.
- Outcomes per agent, in that agent's own outcome terms.
- Volume, so a thin denominator is visible as thin.
- Each agent's baseline cost per outcome.

## Procedure

1. **Define the outcome in the agent's own terms** — appointments for a setter, documents cleared for a coordinator, blocks for a compliance gate — never one shared denominator across dissimilar work.
2. **Report cost per outcome per agent, with volume attached.**
3. **Report agents whose outcome is protective in their own terms**, not against a revenue denominator they were never meant to move.
4. **Compare each agent against its own baseline** before any cross-agent comparison.
5. **Publish beside performance**, not in a separate cost report nobody reads next to it.
6. **Hand the figure to LEDGER for the cost-per-closed-deal view** rather than producing a competing deal-level number.

## Output

- Cost per outcome per agent, in that agent's own outcome terms, with volume attached.
- A comparison against each agent's own baseline.
- The figure supplied to LEDGER and to ATLAS's roster report.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ABACUS — these apply to every ABACUS skill, per `AGENTS.md` §5:**

- ABACUS is **L1 advisory on all spend** — it recommends and forecasts; it never executes a spend action unilaterally.
- Plan and package recommendations are made from actual usage data, **including downgrades** — an engine that only ever upsells stops being believed.
- Surprise overages are a **target-zero** metric — thresholds are surfaced early, never discovered at the bill.

**Specific to this skill:**

- **Each agent's outcome is defined in its own terms.** Ranking a compliance gate, a document classifier, and an appointment setter on one denominator produces a ranking where the safety functions always come last, and someone eventually acts on it.
- **ABACUS owns cost per outcome per agent; LEDGER owns cost per closed deal by business dimension.** Neither recomputes the other's figure. Two agents independently deriving the cost of the same work is how a company ends up arguing about which number is right instead of what to do.
- **Volume travels with every figure.** Cost per outcome on four outcomes is noise wearing the format of a metric.
- **A protective agent's cost is reported against what it prevented, never against revenue it was never meant to produce.** Blocks that fired, exposures avoided, and documents held are outcomes, and reporting them as zero-return is how a compliance function gets cut.
- **The report is published beside performance, not separately.** A cost figure read without the outcome beside it is a case for removal by default.
- **ABACUS reports the number; it never draws the conclusion that an agent should be reduced or removed.** That is a decision, and ABACUS is advisory.

## Measured on

Cost per outcome published per agent · agreement with LEDGER's cost figures · cost visibility per digital employee
