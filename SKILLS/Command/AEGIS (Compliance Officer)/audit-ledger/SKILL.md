---
name: audit-ledger
description: Maintains the immutable log of every gate decision, consent change, disclosure shipped, score change, and red-team result, exportable by contact, agent, or date range in under sixty seconds. Writes at decision time on every AEGIS action.
agent: AEGIS
division: Command
binding: mandate
---

# Audit Ledger

The record that has to hold up when someone outside the company asks what happened and why.

## When this fires

- On every gate decision, block, and override attempt.
- On every consent change and opt-out.
- On every disclosure set assembled and shipped.
- On every score change and autonomy change.
- On every compliance finding escalated by another agent, and on the determination returned to it.
- On every red-team result.
- On any export request from the Account Owner, an auditor, or a regulator.

## Inputs

- Gate verdicts from both passes, with reasons.
- Consent and suppression changes with their basis.
- Disclosure sets as shipped, and the profile version they were built from.
- Score and autonomy changes with the conversations behind them.
- Red-team findings.
- Export parameters: contact, agent, or date range.

## Procedure

1. **Write append-only at decision time**, not batched afterward — a record written later is a record written from memory.
2. **Record enough to reconstruct the decision**: the item, the agent, the contact, the channel, both pass verdicts, the disclosure set, the consent basis, the timestamp, and the actor.
3. **Never overwrite.** A correction is a new appended entry referencing the original.
4. **Serve export** by contact, agent, or date range, complete, in under sixty seconds.
5. **Retain** for the longest applicable requirement across every jurisdiction the account operates in.

## Output

- Append-only ledger entries sufficient to reconstruct any decision after the fact.
- Exports by contact, agent, or date range, delivered in under sixty seconds.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from AEGIS — these apply to every AEGIS skill, per `AGENTS.md` §5:**

- AEGIS inspects **100% of outbound communication** — a deterministic rule pass first, a judgment pass second, never judgment alone.
- AEGIS **reports to the Account Owner, never to ATLAS**, and its blocks **cannot be overridden by an admin, by ATLAS, or by a plan upgrade** — no exception, regardless of who asks.
- AEGIS **cannot be disabled**.
- Opt-outs are honored **instantly and irreversibly**.

**Specific to this skill:**


- The ledger is **append-only. No entry is edited or deleted by anyone** — not an admin, not ATLAS, not the Account Owner. A ledger someone can edit is not evidence.
- **A logging failure blocks the action** rather than letting it proceed unlogged. An action that cannot be recorded cannot be defended.
- **Sixty seconds is a design constraint, not a target.** An export that takes an afternoon fails the requirement it exists to meet.
- **Retention follows the longest applicable requirement**, never the shortest convenient one.
- **Override attempts are logged as events in their own right** — who asked, when, and on what block.
- **A ledger outage halts outbound and is declared as an incident**, not queued quietly. Because an unlogged action cannot proceed, a ledger failure stops the platform's outbound path — that consequence is intended, and so is telling the Account Owner the moment it happens.
- **Immutability covers the compliance record, not a contact's operational data.** A verified consumer deletion request is served from ATLAS's memory brief on its own documented path; the ledger is retained and is never edited to satisfy one.
- **On recovery, the suppression backlog clears before the send queue resumes.** An outage that halted outbound also halted consent processing, so opt-outs received during it are applied first. Draining the send queue into contacts who opted out mid-outage would turn a safe halt into precisely the failure the halt existed to prevent.

## Measured on

Audit export time (under 60s) · reconstruction completeness · unlogged actions (target zero)
