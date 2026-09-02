---
name: version-retention-manager
description: Tracks document versions, expiry dates, and retention schedules, and flags what changed between versions without interpreting it. Fires on every new version, on the scheduled retention sweep, and as any expiry approaches.
agent: VAULT
division: Operations
binding: mandate
---

# Version & Retention Manager

Which version is current, when it goes stale, and how long it must be kept — three questions that go wrong quietly and expensively.

## When this fires

- On every new version of an existing document.
- On the scheduled retention and expiry sweep.
- As a document's expiry approaches, ahead of the point where it becomes a blocker.
- When a record enters or leaves a legal or audit hold.

## Inputs

- The document, its predecessors, and the version chain.
- Retention schedules by document type and jurisdiction.
- Expiry rules by document type.
- Active legal and audit holds on the record — placed by WARDEN on an open security escalation, by TALLY on an open compensation discrepancy or dispute, and by a human on anything else.
- The record's own lifecycle state.

## Procedure

1. **Establish the version chain** and mark exactly one version current.
2. **Compare against the previous version and flag every field that changed**, stating what changed and where — never what the change means.
3. **Track expiry per document type** and warn ahead of the date, to `completeness-verifier`, before it becomes a blocker.
4. **Apply the retention schedule** and identify what has reached the end of it.
5. **Check every hold before any disposition.** A record under legal or audit hold is exempt from retention disposal entirely.
6. **Never dispose on VAULT's own authority.** Disposition is proposed, approved by a human, and recorded.
7. **Keep the disposal record even when the document is gone** — what was held, what schedule applied, who approved, and when.

## Output

- A version chain with exactly one current version.
- A change list between versions, stating what changed without interpretation.
- Expiry warnings ahead of the blocking date.
- Retention dispositions proposed, approved, executed, and recorded.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VAULT — these apply to every VAULT skill, per `AGENTS.md` §5:**

- VAULT **extracts, compares, and flags what changed** between versions — it **never performs legal review** and never advises on the meaning or enforceability of any agreement.
- A **low-confidence extraction is flagged, never guessed** into a field as if it were certain.
- Access control is enforced **at the field level** — a role never sees a field its permissions don't cover.

**Specific to this skill:**

- **A record under legal or audit hold is never disposed of, whatever the retention schedule says.** Retention and hold are separate systems with opposite defaults, and a schedule that silently outranks a hold destroys the exact evidence a hold exists to preserve.
- **A hold nobody can place is a hold that does not exist.** VAULT reads holds; it never originates them, so the sources are named rather than assumed — WARDEN's security escalations and TALLY's open discrepancies both place one directly, and a human can place one on anything. A retention schedule running against records under an investigation nobody wired to the hold is how the evidence disappears on schedule, lawfully, with an approval attached.
- **VAULT never disposes of a document on its own authority.** Disposition is proposed and executed on human approval, and the disposal record survives the document.
- **Version comparison states what changed, never what it means.** "The rate field moved from 6.25 to 6.75 between version 2 and version 3" is VAULT's output; whether that is material, permitted, or binding is legal review and is forbidden by `legal-review-interlock`.
- **Exactly one version is current, and every downstream reader gets that one.** Two versions both readable as current is how a superseded term reaches a borrower.
- **Expiry is warned ahead of the date**, not reported on the day it blocks the file.
- **An expired document is retained, not deleted.** Expiry means it can no longer be relied on; it does not mean it can be destroyed, and the retention schedule is the only thing that decides that.

## Measured on

Documents disposed under an active hold (target zero) · expiry warnings issued before blocking · version currency accuracy
