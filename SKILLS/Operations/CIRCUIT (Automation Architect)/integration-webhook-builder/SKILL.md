---
name: integration-webhook-builder
description: Builds and maintains integrations and webhooks with outside systems, enumerating exactly what data crosses the boundary and failing closed on any credential or endpoint failure. Fires on an integration request, on an error or contract change, and whenever WARDEN rotates or revokes a credential an integration depends on.
agent: CIRCUIT
division: Operations
binding: mandate
---

# Integration & Webhook Builder

Every connection to an outside system is a place where data leaves, credentials live, and failures happen quietly.

## When this fires

- When an integration or webhook is requested.
- When an existing integration errors, times out, or its contract on the other side changes.
- When WARDEN rotates or revokes a credential an integration depends on.
- On the scheduled integration health review.

## Inputs

- The outside system's contract, authentication model, and rate limits.
- The credential reference — held by WARDEN, never by CIRCUIT.
- The field mapping in both directions.
- Retry, rejection, and failure history.
- The exact data leaving the account, field by field.

## Procedure

1. **Enumerate exactly what data crosses the boundary**, in both directions, field by field, before anything is built.
2. **Obtain the credential reference from WARDEN.** CIRCUIT never stores, hardcodes, or copies a secret into a workflow definition.
3. **Map fields explicitly** against the field architecture — never by name-matching that happens to line up today.
4. **Define the failure behavior before the success behavior**: what happens on a timeout, a rejected payload, a rate limit, and a revoked credential.
5. **Hand the integration to [`historical-backtest`](../historical-backtest/SKILL.md) for activation**, exactly as any other workflow. This skill builds and connects; it does not activate.
6. **Register the integration's dependency on its credential**, so a WARDEN rotation or revocation surfaces here rather than as a silent outage.
7. **Document what leaves the account and where it goes**, in plain language.

## Output

- An active integration with its field map in both directions.
- A defined failure behavior for every failure class.
- A registered credential dependency, visible to WARDEN.
- Plain-language documentation of what data crosses the boundary.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **A credential is never stored in a workflow definition, never hardcoded, and never written to a log.** The integration holds a reference; WARDEN holds the secret.
- **An integration fails closed.** A revoked credential, a rejected payload, or an unreachable endpoint stops the integration and reports it — it never falls through to a partial send, an unbounded retry, or a silently skipped step.
- **What leaves the account is enumerated and documented before activation.** An integration whose outbound payload nobody can describe field by field is not built.
- Contact data going to a system that can message that contact **carries the consent and suppression state with it.** Exporting a contact into a tool that can send is a send path, and it is treated as one.
- **An integration is backtested and activated through `historical-backtest` like any other workflow, and this skill never activates one itself.** Being a connection rather than an automation is not an exemption from the gate.
- **A credential rotation or revocation is never worked around** by re-entering the old secret or holding a private copy. The integration breaks visibly and is reconnected properly.

## Measured on

Integration failure rate · silent integration outages (target zero) · request-to-live time
