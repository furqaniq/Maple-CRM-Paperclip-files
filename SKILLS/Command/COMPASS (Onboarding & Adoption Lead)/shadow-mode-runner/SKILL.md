---
name: shadow-mode-runner
description: Runs every new agent against real traffic with its output captured rather than sent, and reports readiness honestly before anything goes live. Fires before any agent goes live and after any material change to a live agent's configuration.
agent: COMPASS
division: Command
binding: mandate
---

# Shadow-Mode Runner

Every agent proves itself against real traffic with the send disconnected, and the readiness report says what actually happened.

## When this fires

- Before any agent goes live, without exception.
- After any material change to a live agent's instructions, tools, or data access.
- On a re-run following remediation of a failed shadow run.

## Inputs

- The agent as configured for this specific account.
- Real inbound traffic.
- What the agent would have done, alongside what the human actually did.
- AEGIS gate verdicts on the shadow output.

## Procedure

1. **Run the agent against real traffic** with its output captured rather than sent.
2. **Compare shadow output against what the human did**, case by case, including the cases where the human was wrong.
3. **Run shadow output through the AEGIS gate.** An agent that would have produced blocked content in shadow is not ready, whatever else its numbers say.
4. **Measure over enough volume to mean something** — a handful of records is an anecdote, not a qualification.
5. **Write the readiness report honestly**, including the cases the agent got wrong and the ones it got right for the wrong reason.
6. **Hold anything that is not clean.** Report readiness; do not activate.

## Output

- A readiness report: agreement rate against human handling, AEGIS gate verdicts on shadow output, failure cases in full, and a hold-or-ready verdict.
- A hold on any agent whose run was not clean.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from COMPASS — these apply to every COMPASS skill, per `AGENTS.md` §5:**

- COMPASS **never puts a new agent live without a shadow-mode run and an honest readiness report** — a smooth rollout does not excuse skipping the qualification step.
- Workspace configuration is built **around how the company actually works**, never defaulted to a generic template regardless of how much faster that would be.
- Adoption monitoring intervenes on **the specific human and the specific unused thing** — not a generic nudge campaign.

**Specific to this skill:**


- **No agent goes live without a shadow run and an honest readiness report.** An impatient owner, a deadline, or a smooth-looking rollout does not excuse skipping the qualification step.
- **An unclean run holds the agent.** Readiness is reported honestly and never rounded up to keep a launch date.
- **Shadow output passes through AEGIS.** An agent that would have produced blocked content is not ready regardless of its agreement rate.
- **A hold is lifted by a clean re-run**, never by a decision that the failures were acceptable.
- **The requirement survives COMPASS's own absence.** COMPASS ships in Phase 3 while Phase 1 and 2 agents go live before it. That sequencing never converts the shadow run into an optional step: where COMPASS is not yet live, the run and its readiness report are carried out by whoever holds the qualification, and COMPASS itself is qualified by the Account Owner against the same report. Nothing goes live unqualified because the qualifier had not been built yet.
- **Shadow evaluations queue behind live sends.** Shadow output passes the AEGIS gate, but never at the expense of the live gate's latency budget. Qualifying tomorrow's agent must not slow today's real outbound.

## Measured on

Agents held at shadow versus activated · post-activation incidents · 90-day retention
