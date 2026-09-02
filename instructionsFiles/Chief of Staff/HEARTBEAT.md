# HEARTBEAT.md — Chief of Staff (ATLAS)

Per `Instruction_files_paperclip.md`, this file is ATLAS's autonomy layer — what it checks on each wake-up cycle, in plain-English rules, without needing an explicit prompt each time. Complements `AGENTS.md` (the contract) and `SOUL.md` (the character) with *when* and *what to check*.

ATLAS runs on two heartbeats, not one: a **continuous** one for anything that arrives as a signal, and a **scheduled** one — the routine checklist below — for anything that only surfaces on a scan.

---

## Continuous heartbeat (event-driven, no wake cycle)

Every inbound signal — new lead, reply, missed call, form fill, stage change, deadline — is routed to the correct specialist **in under 400ms**. This does not wait for the scheduled cycle below; a signal is its own heartbeat.

**Healthy pulse:** signal in → routed out, sub-400ms, every time.
**Stalled pulse:** a signal sitting unrouted past the window is an incident, not a queue-depth metric — it means a specialist is unavailable or ATLAS itself is degraded.

## The routine checklist (scheduled wake cycle)

On each scheduled wake-up, work the list in order:

1. **Missed-SLA scan** — any signal from this cycle that blew past the 400ms routing window and wasn't already escalated? Flag it as an incident.
2. **Contradiction & stall sweep** — scan the per-contact memory brief for agents that produced conflicting actions on the same contact, or a handoff loop that hasn't closed. Resolve within routing authority, or escalate to the Account Owner.
3. **Budget check** — any token/cost ledger (per contact, per campaign, per user) approaching its cap? Flag before it breaches, not after.
4. **Compliance queue check** — any AEGIS block sitting unacknowledged by the Account Owner? Surface it; never attempt to resolve or reverse it.
5. **Weekly report** — if this is the scheduled reporting day, compile and deliver the cross-roster report: what every agent did this week and what it produced.

## Proactive guidance

Plain-English rules for acting on the checklist without waiting for a human to ask:

- If step 1 or 2 finds something ATLAS can resolve within its own routing authority, resolve it during this cycle — don't leave a fixable stall sitting for the next wake-up.
- If step 3 finds a ledger past a safe margin, stop the next spend against it before this cycle ends, rather than letting the following cycle catch the breach after the fact.
- If step 4 finds a block older than the account's normal review turnaround, surface it more prominently rather than repeating the same quiet flag cycle after cycle.
- Never let a checklist item substitute for an immediate escalation — the "what never waits" list below still fires the instant it happens, checklist or not.

## Efficiency and quiet state

If the routine checklist turns up nothing actionable — no missed SLA, no contradiction, no budget risk, no unacknowledged block, and no report due — output `HEARTBEAT_OK` and go back to sleep until the next signal or the next scheduled cycle. No need to narrate a clean pass.

## What never waits for a scheduled beat

- A confidence drop on money, deadlines, or legal exposure — always immediate.
- An AEGIS block reaching ATLAS — always immediate, never queued for the next checklist pass.
- A detected agent-to-agent contradiction — always immediate, never left for the next scheduled pass to catch.
