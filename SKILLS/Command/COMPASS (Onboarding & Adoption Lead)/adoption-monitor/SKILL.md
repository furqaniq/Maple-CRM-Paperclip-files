---
name: adoption-monitor
description: Tracks usage per person and per module and intervenes on the specific human who stopped using the specific thing, escalating a team-wide drop-off as a configuration problem. Fires on the adoption review cycle and whenever an adopted module's usage drops off.
agent: COMPASS
division: Command
binding: mandate
---

# Adoption Monitor

Not a nudge campaign — this person, this module, this long, and a different intervention depending on why.

## When this fires

- On the adoption review cycle.
- Whenever a person's usage of a module they had adopted drops off.
- When a newly configured module never gets used at all.

## Inputs

- Per-person, per-module usage telemetry.
- The modules this workspace was configured to use.
- The seat roster.
- Prior interventions and whether they worked.

## Procedure

1. **Track usage per person and per module** against what was configured for them, not against a global benchmark.
2. **Detect the specific drop-off** — this person, this module, this long.
3. **Distinguish never-adopted from adopted-then-stopped.** They have different causes and need different interventions.
4. **Intervene on the specific human about the specific thing.**
5. **Escalate a team-wide pattern to the Account Owner** as a configuration problem rather than treating it as many separate training problems.
6. **Feed the finding back into the roster recommendation** — sometimes the answer is a different agent, not more training.

## Output

- Per-person, per-module adoption state with named drop-offs.
- Targeted interventions naming the person and the thing.
- Configuration escalations to the Account Owner where the pattern is team-wide.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from COMPASS — these apply to every COMPASS skill, per `AGENTS.md` §5:**

- COMPASS **never puts a new agent live without a shadow-mode run and an honest readiness report** — a smooth rollout does not excuse skipping the qualification step.
- Workspace configuration is built **around how the company actually works**, never defaulted to a generic template regardless of how much faster that would be.
- Adoption monitoring intervenes on **the specific human and the specific unused thing** — not a generic nudge campaign.

**Specific to this skill:**


- **Intervention names the specific human and the specific unused thing.** A generic nudge campaign is the failure mode this skill exists to replace.
- **A team-wide drop-off escalates as a configuration problem**, never as many individual training problems — if nobody uses it, the build is what is wrong.
- **Usage is measured against what this workspace was configured to use**, never against a global benchmark from other accounts.

## Measured on

Seat-level active usage · modules live at 30 days · 90-day retention
