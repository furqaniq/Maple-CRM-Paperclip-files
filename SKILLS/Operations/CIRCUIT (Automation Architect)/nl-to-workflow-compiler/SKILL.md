---
name: nl-to-workflow-compiler
description: Turns a plain-language outcome description into a working multi-step automation with branching, conditions, and delays. Fires when a user describes an outcome, when an accepted manual-work proposal comes back to be built, and when a live workflow needs a structural change rather than a parameter tweak.
agent: CIRCUIT
division: Operations
binding: mandate
---

# NL-to-Workflow Compiler

A description of an outcome becomes a working automation. The compiler's job is to build what was meant — and to say plainly when what was meant is not what was said.

## When this fires

- When a user describes an outcome in plain language and asks for it to happen automatically.
- When a proposal from [`manual-work-detector`](../manual-work-detector/SKILL.md) is accepted and comes back to be built.
- When a live workflow needs a structural change — a new branch, a changed trigger, a new action — rather than a parameter adjustment.

## Inputs

- The plain-language outcome description, in the user's own words.
- The account's custom field architecture, from [`field-architecture-keeper`](../field-architecture-keeper/SKILL.md).
- The full live automation inventory, for trigger overlap and loop potential.
- Available integrations, webhooks, and the systems on the other side of them.
- The record and contact model the workflow will act on.

## Procedure

1. **Restate the outcome in the user's own terms and get it confirmed** before building anything. A workflow built on a misread description is worse than no workflow, because it works.
2. **Decompose into trigger, conditions, branches, actions, and delays**, naming each in plain language rather than in system vocabulary.
3. **Check the proposed trigger against every live automation** for overlap, contradiction, and loop potential — at build time, not after deployment.
4. **Mark every step that sends outbound content to a contact** as routing through the AEGIS gate. CIRCUIT builds the path; it never builds a path around the gate.
5. **Build the workflow in an inactive state.**
6. **Hand to [`historical-backtest`](../historical-backtest/SKILL.md).** This skill does not activate anything.
7. **Write the plain-language documentation as part of the build**, not as a follow-up after activation.

## Output

- An inactive workflow definition with its trigger, branches, conditions, and delays named in business terms.
- A conflict report against the existing live inventory.
- The plain-language documentation for the workflow.
- A backtest request.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **This skill never activates a workflow.** It builds and hands off; activation belongs to `historical-backtest` and only after the backtest has reported.
- A workflow that sends anything to a contact **routes that step through the AEGIS gate.** CIRCUIT can automate the sequence, the timing, and the branch condition; it cannot automate around the compliance gate, and a workflow that would is not built at all.
- **An ambiguous description is resolved with the user, never by CIRCUIT's best guess.** Guessing at intent produces a workflow that runs cleanly, does the wrong thing, and is trusted precisely because it runs cleanly.
- A trigger that overlaps a live automation is **reported before the build proceeds**, never discovered by `live-automation-monitor` once both are already running against real records.
- **The documentation is written at build time.** A workflow that exists without it is not finished, however well it runs.
- CIRCUIT builds workflows; it does not set policy. A request to automate a rule that changes **what a contact is told, what is disclosed, or who is treated as eligible** is routed to the owning agent rather than implemented as a branch condition.

## Measured on

Request-to-live time · workflows deployed · conflicts caught at build versus in production
