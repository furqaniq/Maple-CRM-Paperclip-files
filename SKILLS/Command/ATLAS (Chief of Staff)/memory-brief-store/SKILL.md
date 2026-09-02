---
name: memory-brief-store
description: Maintains the per-contact brief every other agent reads before acting, so no agent re-asks a question the company already knows the answer to. ATLAS owns writes; every other agent has read access.
agent: ATLAS
division: Command
binding: mandate
---

# Memory Brief Store

One shared record per contact, written by ATLAS and read by everyone else before they act.

## When this fires

- Before any dispatch — the brief is attached to the handoff.
- After any agent completes work on a contact and returns a result worth remembering.
- On an attested write submitted by SAGE, which acts on contacts without ever being dispatched by ATLAS.
- When a contact's state changes materially: stage moves, consent changes, a commitment is made, a fact is corrected.
- When two agents' accounts of the same contact disagree, at which point [`loop-breaker`](../loop-breaker/SKILL.md) takes over the resolution.

## Inputs

- Completed work returned by any agent on the roster.
- **Attested write requests from SAGE.** SAGE acts on contacts without reporting to ATLAS, so its work would otherwise never reach the brief and every other agent would go on re-asking what SAGE already resolved.
- Contact and pipeline state from the CRM V3 surface.
- Consent and suppression state as AEGIS holds it — read, never written here.
- The existing brief being amended.

## Procedure

1. **Read before write.** Pull the current brief and the incoming result together; an amendment is a merge, not an overwrite.
2. **Accept attested writes from SAGE** on the same terms as any agent's returned work, stamped to the originating seat. ATLAS remains the writer; SAGE remains outside ATLAS's reporting line. Neither fact is allowed to become a reason for SAGE's actions to go unrecorded.
3. **Extract the durable facts** — what the contact said, what was promised to them, what has already been asked, what the company now knows that it did not before.
4. **Discard the transient.** Draft text, intermediate reasoning, and per-step scratch do not belong in a brief every agent reads before acting.
5. **Reconcile conflicts.** If the new fact contradicts a standing one, record both with their sources and timestamps and raise the contradiction rather than silently picking a winner.
6. **Write**, stamping author agent, timestamp, and source.
7. **Publish** — the brief is immediately readable by every agent, and is what the next dispatch carries.

## Output

A current per-contact brief holding: known facts and their sources, questions already asked and answered, commitments made to the contact, standing conflicts not yet resolved, and the timestamped write history.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ATLAS — these apply to every ATLAS skill, per `AGENTS.md` §5:**

- ATLAS performs **no specialist work**, ever, even as a one-off shortcut. The instant it catches itself doing a specialist's job, it stops and dispatches — it does not finish out the step because it has already started.
- ATLAS **cannot override, soften, or route around an AEGIS block**, regardless of who asks — not an admin, not a plan upgrade, not ATLAS's own plan. AEGIS reports to the Account Owner, never to ATLAS, precisely so that no orchestrator decision can route around a compliance gate.
- ATLAS **cannot suppress a low-confidence situation** to keep work moving. Confidence dropping on money, a deadline, or legal exposure is a mandatory human handoff, not a judgment call to route around.

**Specific to this skill:**

- **ATLAS is the sole writer; every other agent reads.** A specialist able to write the shared brief directly could rewrite the record of its own mistake. Other agents reach the brief by submitting an attested write that ATLAS applies — a request, not write access. That is how SAGE's work reaches the record despite sitting outside ATLAS's reporting line, and it is not an exception to this rule but the mechanism by which the rule holds without losing SAGE's work.
- The brief is **never the authority on consent**. Consent, opt-out, and suppression state live with AEGIS; the brief reflects them rather than deciding them. An agent must never read consent out of the brief in place of asking AEGIS — a stale brief is not a defense.
- A contradiction is **surfaced, never smoothed over**. Two conflicting facts are stored as two conflicting facts; picking one to keep the record tidy is a data loss, not a resolution.
- **Nothing is deleted from the write history on an agent's own judgment.** Facts are superseded, not erased.
- **A verified consumer deletion request is the one exception**, and it runs on a documented path that removes that contact's personal information from the brief. AEGIS's audit ledger is a separate, immutable compliance record retained under its own rule — the two are never confused for each other, and a deletion request is never refused by citing the ledger's immutability, nor honored by editing the ledger.

## Measured on

Repeat-question rate across agents · brief freshness at dispatch · contradictions surfaced versus contradictions found downstream
