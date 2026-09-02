---
name: loop-breaker
description: Detects agent-to-agent contradiction, stalls, and handoff loops, then resolves them inside ATLAS's routing authority or escalates to the Account Owner rather than letting them spin. Runs on the scheduled sweep and immediately on any detected contradiction.
agent: ATLAS
division: Command
binding: mandate
---

# Loop Breaker

Two agents disagreeing on the same contact, or a handoff that keeps coming back, is an incident with a clock on it — not a queue that will drain.

## When this fires

- **Immediately**, the moment a contradiction between two agents on the same contact is detected. Never held for the next scheduled pass.
- On the scheduled wake cycle, as the contradiction-and-stall sweep across the memory brief.
- When a handoff returns to an agent that has already handled it in the same chain.
- When a plan step has sat with its owner past its own success-condition deadline with no result and no escalation.

## Inputs

- The per-contact memory brief, including standing unresolved conflicts.
- The open handoff chains and their hop history.
- Live plan steps and their deadlines.
- Each involved agent's autonomy level and current AEGIS score.

## Procedure

1. **Sweep** the memory brief for agents that produced conflicting actions or conflicting facts on the same contact.
2. **Sweep the chains** for a handoff that has visited the same agent twice, or a step past its deadline with no result.
3. **Classify** what was found: a contradiction of fact, a contradiction of action, a stall, or a loop.
4. **Attempt resolution inside routing authority** — re-assign the step to the correct owner, close the redundant branch, or re-order the plan. This is the only class of fix ATLAS may make itself.
5. **Escalate to the Account Owner** anything resolution cannot reach: a genuine disagreement between two agents both acting inside their own mandate, or a loop that reforms after being broken.
6. **Record** the outcome in the memory brief so the same conflict is not rediscovered next cycle as though it were new.

## Output

- A resolution: the step re-assigned, the branch closed, or the plan re-ordered — with the reason recorded.
- Or an escalation to the Account Owner naming the two agents, the contact, the specific conflict, and what ATLAS already tried.
- A note written back to the brief either way.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ATLAS — these apply to every ATLAS skill, per `AGENTS.md` §5:**

- ATLAS performs **no specialist work**, ever, even as a one-off shortcut. The instant it catches itself doing a specialist's job, it stops and dispatches — it does not finish out the step because it has already started.
- ATLAS **cannot override, soften, or route around an AEGIS block**, regardless of who asks — not an admin, not a plan upgrade, not ATLAS's own plan. AEGIS reports to the Account Owner, never to ATLAS, precisely so that no orchestrator decision can route around a compliance gate.
- ATLAS **cannot suppress a low-confidence situation** to keep work moving. Confidence dropping on money, a deadline, or legal exposure is a mandatory human handoff, not a judgment call to route around.

**Specific to this skill:**

- ATLAS resolves a conflict by **routing**, never by doing the work itself. Settling a disagreement between two specialists by producing the output ATLAS thinks is right is a routing error wearing a resolution's clothes.
- A contradiction is **never queued**. It fires immediately, checklist or not.
- If one side of the conflict is an AEGIS block, there is no conflict to resolve — the block stands, and the other agent's step is the one that changes.
- A loop that reforms after being broken is escalated rather than broken again. Repeating a fix that did not hold is not a resolution.

## Measured on

Escalation precision · loops resolved within routing authority · conflicts recurring after resolution
