---
name: admin-audit-trail
description: Records who changed what, when, and why for every administrative change, with no exceptions and no ability to edit or delete an entry. Fires on every administrative action across every WARDEN skill.
agent: WARDEN
division: Operations
binding: mandate
---

# Admin Audit Trail

Every administrative change, with a reason, permanently. The exceptions are what an audit trail is for, so there are none.

## When this fires

- On every administrative change, from every WARDEN skill and every administrative path.
- On every provisioning, revocation, permission change, structural change, module toggle, and credential action.
- On every access denial and every escalation.
- On any attempt to alter or remove an audit entry.

## Inputs

- The change: what was altered, from what state to what state.
- The actor, including when the actor is an agent rather than a person.
- The timestamp and the stated reason.
- The authorization behind it, where the change required one.

## Procedure

1. **Record before-state and after-state**, not merely that a change occurred.
2. **Record the actor, including which agent acted and on whose instruction.**
3. **Require a reason.** A change without one is recorded as a change without a stated reason, which is itself the finding.
4. **Write the entry as part of the change**, not as a follow-up step that can fail separately.
5. **Never let the trail become the reason a security revocation waits.** Where the primary trail is unreachable, a revocation or credential kill proceeds and its entry is written to the durable fallback, reconciled into the trail on recovery and marked as fallback-written.
6. **Refuse every edit and every deletion.** A correction is a new entry referencing the original; the original stays.
7. **Retain independently of any document retention schedule.**
8. **Make the trail readable and searchable** by the Account Owner without going through WARDEN.

## Output

- A permanent, append-only entry per administrative change: before, after, actor, time, reason, and authorization.
- A correction entry where one is needed, with the original preserved.
- A trail the Account Owner can read directly.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **Every administrative change is recorded, with no exceptions** — not for urgency, not for a seniority level, not for a bulk operation, not for a change made by an agent rather than a person.
- **The trail is append-only. No entry is ever edited or deleted, by anyone, including the Account Owner.** A correction is a new entry that references the original. An audit trail an administrator can edit is a log, and it fails in exactly the case it exists for.
- **The audit entry is written as part of the change, not after it.** A change that succeeds while its audit write fails produces an unrecorded administrative change, which is the failure mode this skill has to make structurally impossible.
- **A revocation is never what waits.** This skill's coupling of change and record would otherwise stop WARDEN killing an exposed credential or a departing user's access whenever the trail is unreachable — and the trail is most likely to be degraded during exactly the incident that makes the revocation urgent. Security revocations proceed on the durable fallback and reconcile afterward. Every other administrative change still blocks on the record: only the revocation path has this exemption, and it is an exemption from *waiting*, never from being recorded.
- **Audit retention is independent of document retention.** VAULT's retention schedules govern documents; they never reach the administrative audit trail, and no disposition proposal may include it.
- **A change made without a stated reason is recorded as exactly that.** Blocking the change is not WARDEN's call in every case; recording the absence always is.
- **Revoking someone's access never removes their audit history.** The record of what a person did outlives their account, permanently.

## Measured on

Administrative changes recorded (target 100%) · audit entries edited or deleted (target zero, any instance an incident) · changes recorded without a stated reason
