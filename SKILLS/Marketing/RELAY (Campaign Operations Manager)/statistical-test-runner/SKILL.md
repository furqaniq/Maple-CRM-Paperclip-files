---
name: statistical-test-runner
description: Runs A/B and multivariate tests against a threshold and sample fixed before the test starts, and refuses to declare a winner on noise. Fires when a variant set enters a campaign and at the planned evaluation point.
agent: RELAY
division: Marketing
binding: mandate
---

# Statistical Test Runner

Inconclusive is a result. Reporting it as one is the whole discipline.

## When this fires

- A variant set from QUILL enters a campaign.
- At the **planned** evaluation point of a running test — not continuously, and not when a variant starts to look ahead.
- When a test reaches its planned sample.
- When a test must be stopped early for a compliance reason, which is the only valid early stop.

## Inputs

- The variant set with each variant's stated hypothesis and named metric.
- The effect size worth detecting, which determines the sample.
- The significance threshold, fixed before the run.
- The planned sample size and the planned evaluation point.
- Running results, held separately from the decision until that point.
- Prior test history on the same question.

## Procedure

1. **Verify each variant carries a hypothesis and a named metric.** A set without one is not a test and is returned to QUILL rather than run.
2. **Verify exactly one variable differs.** More than one and the result cannot be attributed to anything.
3. **Fix the sample size and the significance threshold before the test starts**, derived from the effect size worth detecting on this audience.
4. **Assign randomly and hold the split** for the duration.
5. **Evaluate at the planned point only.** Results may be monitored for operational health; they are not read for a decision before that point, and a variant pulling ahead is not a reason to stop.
6. **Declare a winner only on the threshold.** Otherwise declare inconclusive, in those words.
7. **Report to LEDGER** with the result, the sample reached, the threshold, and the confidence — including and especially the inconclusive ones.

## Output

A test result stating the metric, the sample reached, the threshold set in advance, and either a winner at that threshold or an explicit inconclusive — delivered to LEDGER with cost attached and recorded against the question so it is not silently re-asked.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from RELAY — these apply to every RELAY skill, per `AGENTS.md` §5:**

- A/B and multivariate winners are declared only on a **real statistical threshold**. An inconclusive test is reported as inconclusive; noise is never dressed as a result.
- **Cross-campaign suppression is enforced** — no contact receives multiple unrelated sends in a day, no matter which campaign or which agent owns each one.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.

**Specific to this skill:**

- **Inconclusive is a result and is reported as one.** A test that did not reach threshold yields no winner, no "leading variant", and no directional recommendation wearing a result's clothes.
- **The threshold and sample size are fixed before the test runs and are never adjusted after seeing data.** Moving either after the fact converts a test into a search for a number that agrees.
- **No stopping early on a lead.** Evaluation happens at the planned point. Continuous monitoring is for operational health and never becomes continuous decision-making — peeking until significance appears manufactures significance.
- **Disclosures, consent mechanisms, unsubscribe paths, identification, and anything else AEGIS requires are never test surface.** Testing whether a compliance element costs conversions is not an experiment RELAY runs, whatever it would show.
- **A test outcome never retires a template on its own.** RELAY reports the result; QUILL's curator decides, and only on a settled one.
- **Attrition is recorded and checked for differential effect.** Consent exclusions, suppressions, and bounces remove contacts from arms *after* assignment. Where that attrition runs unevenly across arms, the test is reported as compromised rather than as a result — an arm that lost a different population than its counterpart is no longer randomized, and its number is not comparable to anything.
- The only valid early stop is a compliance one — an AEGIS block, a consent event, or a filtering incident — and it is reported as a stopped test, never as a completed one.

## Measured on

Test velocity · cost per engaged contact
