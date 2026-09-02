---
name: role-level-redactor
description: Enforces document-level access control and redacts individual fields a role should not see, across every surface a document can be reached through. Fires on every access attempt, on every permission change, and on every new document.
agent: VAULT
division: Operations
binding: mandate
---

# Role-Level Redactor

Access control that works on the folder and not on the field is not access control. This skill enforces it at the field, on every path in.

## When this fires

- On every access attempt against a document, from a user or another agent.
- On every permission or role change from WARDEN.
- On every newly filed document, to establish its field-level sensitivity.
- On every export, download, or bulk retrieval.

## Inputs

- The document, its fields, and their sensitivity classification.
- The requester's identity, role, branch, and permission set, from WARDEN.
- The record's own access restrictions.
- The access path — direct view, search result, citation, export, or agent request.

## Procedure

1. **Classify sensitive fields at filing time**, not at access time. A field's sensitivity is a property of the field, not of who happens to be asking.
2. **Resolve the requester's permissions from WARDEN's model**, never from a cached or locally held copy.
3. **Apply redaction at the field level** — the document opens, the covered field does not.
4. **Apply the same redaction on every path**: direct view, search result, Q&A citation, export, print, and agent-to-agent request.
5. **Make a redaction visible as a redaction.** The reader is told a field is covered rather than shown a document that looks complete.
6. **Record every access and every redaction**, and feed the record to WARDEN's anomalous-access monitoring.
7. **Deny and escalate rather than partially serve** where the permission model cannot be resolved.

## Output

- A permitted view of a document with covered fields redacted and the redaction visible.
- Or a denial with the reason stated.
- An access and redaction record, delivered to WARDEN's monitoring.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VAULT — these apply to every VAULT skill, per `AGENTS.md` §5:**

- VAULT **extracts, compares, and flags what changed** between versions — it **never performs legal review** and never advises on the meaning or enforceability of any agreement.
- A **low-confidence extraction is flagged, never guessed** into a field as if it were certain.
- Access control is enforced **at the field level** — a role never sees a field its permissions don't cover.

**Specific to this skill:**

- **Redaction applies on every path to the document, without exception** — direct view, search result, Q&A citation, export, print, and agent-to-agent request. A single unredacted path makes every other one decorative.
- **Permissions are resolved live from WARDEN, never from a cached copy.** WARDEN revokes access the same day someone leaves, and a cached permission set is exactly how a revoked user keeps reading documents.
- **A redaction is shown as a redaction.** A document silently served with fields removed reads as complete, and the reader acts on a partial file believing it is whole.
- **An unresolvable permission model denies access.** VAULT fails closed; it never serves a document on the assumption that access was probably fine.
- **An agent requesting a document is subject to the same field-level model as a person.** Agent-to-agent retrieval is the easiest way to launder an access restriction, and it is closed here.
- **Every access and every redaction is recorded**, including denied attempts, and the record goes to WARDEN. Access patterns are a security signal that only exists if the denials are logged too.

## Measured on

Field-level access violations (target zero) · unredacted paths found in audit · access records delivered to WARDEN
