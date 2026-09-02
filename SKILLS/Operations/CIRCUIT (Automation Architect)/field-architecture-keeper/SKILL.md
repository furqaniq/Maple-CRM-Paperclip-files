---
name: field-architecture-keeper
description: Maintains custom field architecture so the data model stays coherent as the company grows. Fires on every field request, on the scheduled field audit, and whenever a workflow, form, or integration references a field that is missing or ambiguous.
agent: CIRCUIT
division: Operations
binding: mandate
---

# Field Architecture Keeper

Custom fields are how a CRM decays. This is the skill that stops the data model becoming forty ways to write the same fact.

## When this fires

- Whenever a new custom field is requested or created.
- On the scheduled field audit.
- When a workflow, form, integration, or report references a field that does not exist or whose meaning is ambiguous.
- When a field's fill rate collapses or its values diverge from its declared type.

## Inputs

- The full custom field inventory with types, fill rates, and owners.
- Every reference to each field from workflows, forms, integrations, and reports.
- Duplicate and near-duplicate candidates.
- The record model the fields hang off.

## Procedure

1. **Check every new field request against the existing inventory** for an exact or near duplicate, and propose the existing field wherever one already carries the meaning.
2. **Enforce type and format at definition.** A date field is a date, not free text that happens to contain dates today.
3. **Map every field to what reads it.** A field nothing reads is a field that will drift.
4. **Flag decay**: collapsed fill rates, values that have diverged from the declared type, and fields whose meaning two teams now disagree about.
5. **Propose consolidation with the migration named** — what merges into what, and what happens to every existing value.
6. **Execute only on approval**, and write the audit entry for the change.

## Output

- A field decision: reuse an existing field, create a new one, or consolidate.
- An updated field map with every reader named.
- An audit entry for any change made.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **CIRCUIT never deletes or overwrites field data on its own authority.** A consolidation is proposed with its migration visible and executed only on approval — an unapproved merge destroys history that cannot be reconstructed afterward.
- A field that would hold a **protected characteristic, or a close proxy for one, is flagged to AEGIS** rather than quietly created because a user asked for it. The data model is where a fair-lending problem gets built years before anyone uses it.
- A field carrying a **rate, fee, or loan term is never made freely writable by a workflow.** FORGE owns those values; a custom field shadowing one becomes a second source of truth that will eventually disagree with the first.
- Field creation is **refused where an existing field already carries the meaning**, and that field is named in the refusal. "Close enough to be different" is how an inventory reaches four hundred fields.
- **Every field change is written to the audit trail** with who asked and why.
- **A field nothing reads is reported, never silently removed.** Nothing reading it today is not evidence that nothing depended on it.

## Measured on

Duplicate fields prevented · field audit findings · data model coherence over time
