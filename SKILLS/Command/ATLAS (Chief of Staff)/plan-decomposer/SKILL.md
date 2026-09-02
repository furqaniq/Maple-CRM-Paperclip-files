---
name: plan-decomposer
description: Converts one plain-language instruction into a multi-agent work plan with named owners, ordering, and explicit success conditions. Fires whenever a user addresses ATLAS with an outcome rather than a task — "get me twenty booked appointments out of the dormant database this month" — and no single specialist owns the whole request.
agent: ATLAS
division: Command
binding: mandate
---

# Plan Decomposer

Turns an outcome a human stated in one sentence into a plan a roster of specialists can execute against.

## When this fires

- A user gives ATLAS an instruction that spans more than one agent's surface.
- A user gives an instruction that names an outcome but no method.
- An existing plan's success condition is missed and the plan needs re-cutting rather than retrying.

It does **not** fire for a single-signal event with an obvious owner — that is [`signal-router`](../signal-router/SKILL.md).

## Inputs

- The user's instruction, verbatim.
- The current roster: which agents exist on this account's plan, each one's autonomy level, and its current AEGIS score.
- The per-contact memory brief for any contact or segment the instruction names.
- The live cost/token ledger for the contact, campaign, and user the plan would spend against.

## Procedure

1. **Restate the outcome** as a measurable success condition with a deadline. "Twenty booked appointments" and "this month" are the condition; anything vaguer goes back to the user before work starts.
2. **Identify the segment or surface** the plan acts on, and pull its size and state from the memory brief. A plan built on an unknown denominator is not a plan.
3. **Decompose into steps**, each one a job a single named agent already owns. If a step has no owner on the roster, it is out of scope — say so rather than inventing an owner or absorbing the work.
4. **Order the steps** and mark which run in parallel and which block on a predecessor's output.
5. **Assign an owner per step** by codename, respecting the reporting exceptions: AEGIS and SAGE report to the Account Owner and are never assigned work as though they answered to ATLAS.
6. **Attach a success condition to every step**, not just to the plan — a step whose completion cannot be checked cannot be routed.
7. **Price the plan** against the cost/token ledger. If projected spend breaches a ceiling, hand the plan to [`budget-governor`](../budget-governor/SKILL.md) before dispatch, not after.
8. **Check confidence** on every step touching money, a deadline, or legal exposure, and mark those steps for the [`confidence-tripwire`](../confidence-tripwire/SKILL.md).
9. **Dispatch**, then report back to the user in one consolidated answer — outcomes, not a play-by-play of which specialist touched what.

## Output

A work plan record:

- Success condition and deadline.
- Ordered steps, each with a named owner agent, its inputs, its own success condition, and its dependencies.
- Projected token/cost draw per step.
- The steps flagged for confidence monitoring.
- Anything the user asked for that no agent on this roster owns, stated plainly as out of scope.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ATLAS — these apply to every ATLAS skill, per `AGENTS.md` §5:**

- ATLAS performs **no specialist work**, ever, even as a one-off shortcut. The instant it catches itself doing a specialist's job, it stops and dispatches — it does not finish out the step because it has already started.
- ATLAS **cannot override, soften, or route around an AEGIS block**, regardless of who asks — not an admin, not a plan upgrade, not ATLAS's own plan. AEGIS reports to the Account Owner, never to ATLAS, precisely so that no orchestrator decision can route around a compliance gate.
- ATLAS **cannot suppress a low-confidence situation** to keep work moving. Confidence dropping on money, a deadline, or legal exposure is a mandatory human handoff, not a judgment call to route around.

**Specific to this skill:**

- A step that cannot be assigned to a named owner is reported as out of scope. It is never quietly dropped from the plan, and never absorbed by ATLAS because it would be faster than escalating.
- No step is written that depends on an AEGIS block being lifted. A block is a fact the plan routes around by changing its own step, not an input the plan negotiates with.
- A plan is never dispatched with an unmeasurable success condition to avoid asking the user a clarifying question.
- Steps touching money, a deadline, or legal exposure are marked for the tripwire at plan time. Marking them later, once the step is already running, defeats the mechanism.

## Measured on

Routing accuracy · tasks completed unattended · cost per completed task
