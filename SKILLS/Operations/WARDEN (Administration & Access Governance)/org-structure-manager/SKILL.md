---
name: org-structure-manager
description: Maintains branch and team structure, routing rules, and territory assignment as the organization changes. Fires on any structural change and whenever routing points at a seat that no longer exists.
agent: WARDEN
division: Operations
binding: mandate
---

# Org Structure Manager

The org chart is not a diagram — it is the routing table for every lead, every record, and every permission in the account.

## When this fires

- On any branch, team, or reporting change.
- On a territory reassignment or split.
- When routing points at a seat that no longer exists or a person who has left.
- On the scheduled structure review.
- When LEDGER surfaces a dead source, a collapsed contact rate, or a stalled stage whose shape points at routing rather than at demand.

## Inputs

- Current branch, team, and reporting structure.
- Territory definitions and their assignments.
- Routing rules that depend on the structure.
- Records, pipelines, and in-flight work attached to any seat being changed.
- Licensing coverage per territory, where the territory implies a licensed activity.

## Procedure

1. **Model the change before applying it**, including everything downstream that routes off the structure.
2. **Identify every routing rule, permission, and report that depends on the part being changed.**
3. **Check licensing coverage** where a territory change would route activity into a state or region nobody assigned holds a licence for.
4. **Reassign in-flight work explicitly.** Records mid-process do not silently transfer with a territory.
5. **Apply the change and verify no routing rule now points at nothing.**
6. **Notify the affected people and the agents whose routing changed.**
7. **Write the audit entry** naming the structural change and everything it moved.

## Output

- An updated branch, team, and territory structure.
- Rebuilt routing rules with no dangling targets.
- Explicit reassignment of in-flight work.
- An audit entry naming the change and its downstream effects.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **A structural change is modelled before it is applied.** Territory splits are where leads silently stop being routed to anyone, and the failure is invisible until someone notices the pipeline is thin.
- **No routing rule is left pointing at a seat that does not exist.** A dangling route drops leads without erroring, which is the worst available failure mode.
- **Licensing coverage is checked before a territory is assigned.** Routing activity into a jurisdiction where nobody on the receiving team is licensed creates a regulatory exposure out of an administrative change.
- **In-flight work is reassigned explicitly, never carried implicitly with a territory.** A contact mid-conversation whose owner changes without anyone being told gets two follow-ups or none.
- **Structure changes are announced to the people and agents affected**, not applied silently and discovered.
- **Every structural change is written to the audit trail** with who requested it and why. Structure changes are permission changes wearing a different name.
- **A LEDGER anomaly shaped like a routing failure is checked here, not left as an unexplained metric.** LEDGER detects that leads stopped arriving and is advisory by design; WARDEN owns the routing table that would explain it. Neither half is a finding on its own, and a dangling route reads exactly like a dead channel for as long as nobody connects them.

## Measured on

Routing rules with dangling targets (target zero) · leads dropped after a structural change (target zero) · permission audit findings originating from structure changes
