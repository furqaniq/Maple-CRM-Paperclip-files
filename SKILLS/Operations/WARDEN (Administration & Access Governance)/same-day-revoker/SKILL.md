---
name: same-day-revoker
description: Removes access the same day someone leaves, names every dependency the revocation breaks, and preserves the departing user's audit history intact. Fires the moment a departure is known, never on a schedule.
agent: WARDEN
division: Operations
binding: mandate
---

# Same-Day Revoker

Same day, no exceptions. The only question is what breaks when access ends, and that question is answered out loud rather than left to fail quietly.

## When this fires

- The moment a departure is known — resignation, termination, or end of engagement.
- On a contractor seat reaching its expiry.
- On an immediate-risk departure, ahead of any notice period.
- Never on a batch or a scheduled sweep.

## Inputs

- The departing person, their access set, and their effective date.
- Every dependency their access holds: API tokens, integrations, owned automations, routing assignments, and records they own.
- Their audit history and the records attributed to them.
- The receiving owner for anything they held.

## Procedure

1. **Revoke access the same day.** This step does not wait on the dependency review below it.
2. **Revoke and rotate every credential they held or could have seen**, rather than transferring it.
3. **Enumerate what the revocation breaks** — integrations authenticated as them, automations they owned, routing that pointed at them — and name each to a receiving owner.
4. **Reassign ownership of records, pipelines, and in-flight work** so nothing is left owned by a revoked account.
5. **Preserve the audit history in full.** Revoking access never removes what the person did.
6. **Confirm zero residual access** across every module, integration, and token, and check the account against the orphaned-account target.
7. **Write the audit entry**, including the dependencies broken and where each was routed.

## Output

- A revoked account with zero residual access, same day.
- Rotated credentials for everything the person held or could see.
- A named list of broken dependencies, each routed to a receiving owner.
- A preserved audit history and reassigned record ownership.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **Revocation happens the same day and is never deferred, batched, or held pending a dependency review.** The dependency work runs alongside revocation, never ahead of it — an account left open because an integration depends on it is the single most common way a departed user retains access for months.
- **Breakage is named, never traded against.** An integration or automation that stops working when access ends is reported immediately to a named owner. What is unacceptable is not the breakage; it is the breakage nobody was told about, which CIRCUIT then finds as a silent failure days later.
- **Credentials are rotated, not transferred.** Handing a departing person's token to their replacement means the departed person still holds a working credential, and the audit trail now attributes the successor's actions to a shared secret.
- **The audit history is preserved in full.** Revoking access never removes, anonymizes, or detaches what the person did — the record of their administrative changes outlives their account without exception.
- **Nothing is left owned by a revoked account.** Records, pipelines, automations, and routing all get a living owner on the same day, or the work they represent silently stops.
- **An immediate-risk departure is revoked before the notice, not after it.** Notice periods are an HR arrangement; they are not an access-control policy.
- **An unavailable audit trail never delays revocation.** Access ends the same day and the entry lands on the durable fallback, per `admin-audit-trail`. Nothing about recording the revocation is allowed to become a reason it has not happened yet.

## Measured on

Orphaned accounts (target zero) · same-day revocation compliance (target 100%) · dependencies broken silently (target zero)
