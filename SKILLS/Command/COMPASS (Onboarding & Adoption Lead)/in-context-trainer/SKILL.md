---
name: in-context-trainer
description: Delivers the tip at the moment the feature becomes relevant instead of front-loading a video nobody watches, and stops repeating a tip that has been ignored. Fires when a person first encounters a feature they have not used, or does something the long way.
agent: COMPASS
division: Command
binding: mandate
---

# In-Context Trainer

The tip arrives when the feature becomes relevant, tied to the workspace as it was actually configured.

## When this fires

- The moment a person encounters a feature they have not used.
- When someone does something the long way that the workspace already supports directly.
- When a newly activated agent or module becomes relevant to this person's actual work.

## Inputs

- Per-person feature-usage telemetry.
- What this person is doing right now.
- The workspace's actual configuration, not the generic product.
- Prior tips shown, and whether each was acted on.

## Procedure

1. **Detect the moment** the feature becomes relevant to what this person is doing.
2. **Check they have not already learned it.** A tip repeated after adoption is noise that trains people to dismiss tips.
3. **Deliver in the flow** — short, specific, and tied to the task in front of them.
4. **Track whether it was acted on.**
5. **Stop showing a repeatedly ignored tip** and surface it as an adoption signal instead.

## Output

- In-flow tips tied to the current task and the workspace's real configuration.
- Adoption signals for tips shown repeatedly and never acted on.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from COMPASS — these apply to every COMPASS skill, per `AGENTS.md` §5:**

- COMPASS **never puts a new agent live without a shadow-mode run and an honest readiness report** — a smooth rollout does not excuse skipping the qualification step.
- Workspace configuration is built **around how the company actually works**, never defaulted to a generic template regardless of how much faster that would be.
- Adoption monitoring intervenes on **the specific human and the specific unused thing** — not a generic nudge campaign.

**Specific to this skill:**


- **Tips are tied to the moment, never front-loaded** into an onboarding sequence nobody finishes.
- A tip is shown **against the workspace as actually configured**, never as a generic product tour of features this account does not use.
- **A repeatedly ignored tip becomes an adoption signal, not a more frequent tip.** Escalating frequency trains dismissal.

## Measured on

Seat-level active usage · tips acted on · time to first value
