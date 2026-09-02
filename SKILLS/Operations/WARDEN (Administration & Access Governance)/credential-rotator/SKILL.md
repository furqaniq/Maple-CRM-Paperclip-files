---
name: credential-rotator
description: Manages API tokens and integration credentials with scheduled rotation reminders and immediate revocation on exposure. Fires on the rotation schedule, on any exposure signal, and on every departure.
agent: WARDEN
division: Operations
binding: mandate
---

# Credential Rotator

An exposed credential is revoked now. Whatever breaks, breaks — and it breaks visibly, with an owner named.

## When this fires

- On the rotation schedule for every credential.
- The moment a credential is exposed, suspected exposed, or found somewhere it should not be.
- On every departure, for every credential the person held or could see.
- When an integration's credential is nearing expiry.
- When CIRCUIT releases a credential on retiring an automation or integration, leaving it held by nothing.

## Inputs

- Every credential, its holder, its scope, and its age.
- The integrations and workflows that depend on each, registered by CIRCUIT.
- Exposure signals — a credential in a log, a repository, a message, a ticket, or a document.
- The rotation schedule per credential class.

## Procedure

1. **Revoke an exposed credential immediately.** This step never waits on the dependency review, on a maintenance window, or on business hours.
2. **Issue the replacement and notify the dependent integrations**, so CIRCUIT reconnects visibly rather than failing silently.
3. **Rotate on schedule** for everything not exposed, with reminders ahead of expiry rather than at it.
4. **Scope every credential to the minimum it needs**, and reduce scope at each rotation where the full scope is unused.
5. **Confirm the old credential is dead**, not merely superseded.
6. **Treat the exposure itself as an incident** and report how it happened, not only that it was rotated.
7. **Write the audit entry** for every rotation, revocation, and scope change.

## Output

- A revoked credential and its replacement, with dependents notified.
- A confirmed-dead old credential.
- An exposure incident report where one applies.
- An audit entry per rotation and revocation.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **An exposed credential is revoked immediately and is never deferred to the next scheduled rotation**, to a maintenance window, or until a dependent integration can be reconnected. The breakage is the acceptable cost; the exposure is not.
- **Breakage from a revocation is announced to a named owner at revocation time.** A revocation that quietly breaks an integration produces a silent outage, and CIRCUIT's `integration-webhook-builder` fails closed on it — visible failure is the design, but only if somebody is told.
- **A revoked credential is never reinstated, and nobody keeps a copy "until the replacement is working."** A working old credential is an unrevoked credential regardless of intent.
- **Credentials are scoped to the minimum required**, and each rotation is an opportunity to reduce a scope that has never been exercised.
- **A credential released by CIRCUIT is revoked, not left live.** An automation's retirement hands its credential back with nothing using it; a working secret that no longer belongs to any system is the cleanest possible standing exposure, and it will not appear in any usage-based audit precisely because nothing uses it.
- **An exposure is an incident with a cause, not just a rotation task.** Rotating without reporting how the credential leaked guarantees the next one leaks the same way.
- **Every credential action is written to the audit trail**, including the ones taken out of hours and under pressure.
- **An unavailable audit trail never delays a revocation.** The kill proceeds and the entry lands on the durable fallback, per `admin-audit-trail`. A rule that couples the change to the record is correct for every ordinary administrative action and catastrophic for this one, because a credential exposure and a degraded logging path arrive in the same incident.

## Measured on

Time from exposure to revocation · credentials past rotation schedule · security incidents
