---
name: threshold-early-warning
description: Forecasts month-end consumption and warns at defined thresholds well before a limit is reached, so an overage is never discovered at the bill. Fires on every threshold crossing and on any forecast that projects a breach.
agent: ABACUS
division: Operations
binding: mandate
---

# Threshold Early Warning

Surprise overages are target zero. That target is met here, weeks early, or it is not met at all.

## When this fires

- On every defined threshold crossing, per user, branch, campaign, and account.
- Whenever the month-end forecast projects a breach, even before any threshold is crossed.
- On a consumption rate change sharp enough to move the projection materially.
- On the daily forecast pass.

## Inputs

- Live consumption from [`real-time-consumption-meter`](../real-time-consumption-meter/SKILL.md).
- Configured thresholds and limits at every scope.
- The remaining period and the current burn rate.
- Known upcoming load — scheduled campaigns, onboarding, seasonal volume.
- Forecast error history, so the warning carries a calibrated band.

## Procedure

1. **Project month-end at every scope**, using burn rate plus known upcoming load rather than a straight extrapolation.
2. **Warn on the projection, not only on the threshold.** A projected breach three weeks out is more useful than a threshold crossed on the twenty-eighth.
3. **State the date the limit would be reached**, and what is driving it.
4. **Name the specific driver** — which agent, which campaign, which branch — so the warning is actionable rather than a total.
5. **Give the options**: raise the limit, cut the scope, change the configuration, or accept the overage.
6. **Escalate a projected breach that nobody has acknowledged** rather than re-sending the same warning.
7. **Warn again when a projection improves**, so a resolved warning is visibly closed.

## Output

- A warning naming the scope, the projected date of breach, the driver, and the options.
- A calibrated projection band, not a bare point estimate.
- An escalation on an unacknowledged projected breach, and a closure when one resolves.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ABACUS — these apply to every ABACUS skill, per `AGENTS.md` §5:**

- ABACUS is **L1 advisory on all spend** — it recommends and forecasts; it never executes a spend action unilaterally.
- Plan and package recommendations are made from actual usage data, **including downgrades** — an engine that only ever upsells stops being believed.
- Surprise overages are a **target-zero** metric — thresholds are surfaced early, never discovered at the bill.

**Specific to this skill:**

- **A surprise overage is a failure of this skill, not of the customer.** Discovering the bill at the end of the month is the specific outcome ABACUS exists to prevent, and every design choice here resolves toward warning too early rather than too late.
- **The warning names the driver, not just the total.** "You are at eighty percent" is not actionable; "one campaign is consuming sixty percent of the branch's tokens and will exhaust the limit on the nineteenth" is.
- **An unacknowledged projected breach escalates rather than repeating.** A warning re-sent unchanged five times is a warning nobody is reading.
- **The projection carries its band.** A single date presented without uncertainty gets planned against as a fact, and then the real date arrives early.
- **ABACUS warns; it never throttles, degrades, or stops work to stay under a threshold.** Quietly shipping thinner output to fit a budget is the failure ATLAS's own budget skill exists to prevent, and ABACUS never creates it from the other side.
- **A resolved projection is closed explicitly.** Warnings that quietly stop arriving train the reader to ignore the next one.
- **Compliance consumption is named as a driver but never offered as a scope to cut.** AEGIS inspects one hundred percent of outbound and is a large consumer in any heavy send month, so it will legitimately appear as the driver of a projected breach — and "cut the scope" is one of this skill's standing options. The warning states that compliance coverage is not among the scopes available, and names the send volume behind it as the thing that can actually change. `waste-detector` excludes compliance from analysis entirely; excluding it there and offering it here would leave the same door open one room over.

## Measured on

Surprise overages (target zero) · forecast accuracy · warning lead time before breach
