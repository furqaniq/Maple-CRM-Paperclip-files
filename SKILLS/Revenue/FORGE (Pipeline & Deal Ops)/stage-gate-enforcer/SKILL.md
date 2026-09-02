---
name: stage-gate-enforcer
description: Blocks an invalid stage transition and names the specific unmet requirement rather than failing silently. Fires on every attempted stage change, from any source.
agent: FORGE
division: Revenue
binding: mandate
---

# Stage Gate Enforcer

A blocked transition that does not say what is missing is indistinguishable from a broken system, and gets routed around within a week.

## When this fires

- On every attempted stage transition, whether initiated by a user, an agent, or an automation.
- On a change to a stage's entry or exit criteria, against files already sitting in it.
- On a human override of a block.

## Inputs

- The file, its current stage, and the requested transition.
- The stage's entry and exit criteria, as configured for this account.
- Document completeness from VAULT's `completeness-verifier`.
- The requester's identity and role.

## Procedure

1. **Evaluate every criterion for the requested transition**, not only the first that fails.
2. **Allow the transition where all criteria are met.**
3. **Block and name every unmet requirement**, specifically, with what would satisfy each one — never a generic refusal.
4. **State who can satisfy each requirement**, so the block routes to an action rather than to a support ticket.
5. **Permit a human override, and record it** — who overrode, when, and which requirement was waived.
6. **Re-evaluate files already in a stage when its criteria change**, and surface the ones that would no longer qualify rather than silently retro-blocking them.
7. **Never alter a requirement to let a transition through** — see [`terms-interlock`](../terms-interlock/SKILL.md).

## Output

- An allowed transition, or a block naming every unmet requirement and what satisfies each.
- A recorded override, naming the human and the waived requirement.
- A re-evaluation report where stage criteria changed under files already in the stage.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from FORGE — these apply to every FORGE skill, per `AGENTS.md` §5:**

- FORGE **never alters terms, rates, locks, or fees.** It surfaces, chases, notifies, and escalates. Changes are human acts with human audit trails.
- Document chasing **always checks the file vault first** before re-requesting from the customer.
- Missed deadlines are a **target-zero** metric — the 72/48/24-hour escalation ladder is mandatory, not optional.

**Specific to this skill:**

- **A block names every unmet requirement at once, not the first one.** Revealing them one at a time turns a five-minute fix into five round trips, and the producer learns to override rather than to comply.
- **A block is never silent.** A transition that simply does not happen looks like a bug, and the fix a user finds for a bug is a workaround.
- **A human may override; the system may not.** Every override is recorded with who, when, and what was waived — an unrecorded override is a stage criterion that quietly does not exist.
- **FORGE never modifies a requirement to satisfy a gate.** Changing what the stage requires so the file can pass is the same act as altering the file's terms, performed one level up.
- **Document completeness comes from VAULT, not from FORGE's own reading.** VAULT owns what is present, legible, and current, and a second determination in FORGE will disagree with it exactly when a document is borderline.
- **A criteria change never silently retro-blocks a file mid-stage.** Files that would no longer qualify are surfaced for a decision, because a file that moved backwards overnight is a customer promise that broke with no one deciding to break it.
- **A gate result is never communicated to a customer as an outcome.** "You've been approved to move to underwriting" is an eligibility statement wearing a pipeline stage.

## Measured on

Invalid transitions blocked · blocks naming every unmet requirement (must be 100%) · silent failures (target zero) · overrides recorded with a named human
