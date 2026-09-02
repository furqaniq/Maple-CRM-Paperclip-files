---
name: hypothesis-tagged-variants
description: Ships each copy variant with a stated, falsifiable hypothesis and a named metric attached, so RELAY's test measures something real and LEDGER can interpret it. Fires whenever more than one version of an asset is produced for the same audience and objective.
agent: QUILL
division: Marketing
binding: mandate
---

# Hypothesis-Tagged Variants

A variant without a hypothesis is not a test, it is two guesses in a trench coat.

## When this fires

- More than one version of an asset is produced for the same audience and the same objective.
- RELAY requests a variant set for a campaign test.
- A template's performance is being challenged and a competing version is written.

## Inputs

- The base asset and the objective it serves.
- The single variable to be tested and the reason it is worth testing.
- The audience and its size, which bounds what is detectable at all.
- The metric the hypothesis will be judged on.
- RELAY's test configuration: significance threshold, sample size, and planned evaluation point.
- Prior test history, so a settled question is not re-asked.

## Procedure

1. **Name the one variable** that differs between variants. If more than one differs, this is not a test and is not shipped as one.
2. **State the hypothesis in falsifiable terms**, naming the metric and the direction expected — "a benefit-led subject line raises open rate against a curiosity-led one", not "see which does better."
3. **Hold everything else constant** across the set — offer, audience, send window, disclosure, creative.
4. **Verify each variant independently** through the compliance filter. A variant is never covered by a sibling's pass.
5. **Verify each variant carries its own complete disclosure.** Disclosures are not test surface and are identical across the set.
6. **Emit the set** tagged with the hypothesis, the variable, the metric, and the audience.
7. **Hand to RELAY's `statistical-test-runner`.** QUILL writes variants; it does not run tests and does not declare outcomes.

## Output

A variant set in which exactly one variable differs, each variant carrying a stated falsifiable hypothesis, the named metric, its own complete disclosure block, and a link to the base asset and prior tests on the same question.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **Every variant carries a stated, falsifiable hypothesis.** "See which performs better" is not a hypothesis, and a set shipped without one is returned rather than tested.
- **More than one variable changed is not a test.** Such a set may still ship as two assets, but never as a test, and its result is never reported as evidence about either variable.
- **QUILL never declares a winner.** That is RELAY's threshold and RELAY's call. QUILL also never treats a result RELAY reported as inconclusive as though it settled anything — including as grounds to retire a template.
- **Disclosures, consent mechanisms, and unsubscribe paths are never varied.** Anything AEGIS requires is identical across the set; testing it is testing compliance.
- Each variant is independently complete and independently screened. A set is never cleared as a set.

## Measured on

Engagement lift per variant · compliance rejection rate under 2% · turnaround time
