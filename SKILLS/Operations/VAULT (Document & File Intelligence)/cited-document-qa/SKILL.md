---
name: cited-document-qa
description: Answers questions across the document set with the source cited, within the asker's own field-level permissions. Fires whenever a user or another agent asks a question the document set can answer.
agent: VAULT
division: Operations
binding: mandate
---

# Cited Document Q&A

An answer without a citation is a claim. An answer that cites a document the asker is not cleared to see is a leak wearing a citation.

## When this fires

- Whenever a user asks a question the document set can answer.
- Whenever another agent needs a fact that lives in a document rather than a field.
- When a human is preparing for a conversation and needs what the file actually says.

## Inputs

- The question, and the identity and role of whoever asked it.
- The asker's field-level and document-level permissions.
- The document set within the records the asker can see.
- Version state — which version of each document is current.

## Procedure

1. **Resolve the asker's permissions first**, before searching anything. The permitted document set is the search space; it is not a filter applied to results afterward.
2. **Search within that space only.**
3. **Answer with the source cited** — document, version, and page — so the reader can verify rather than trust.
4. **Say plainly when the document set does not answer the question.** An unanswerable question gets "the documents do not say," never an inference presented as a finding.
5. **Name the version.** An answer drawn from a superseded version is stated as such.
6. **Refuse interpretation.** Where the answer would require saying what a clause means or whether it binds, return what the document states and route the interpretation to a human.
7. **Record the query and what was returned** in the access record.

## Output

- An answer with document, version, and page cited.
- Or an explicit statement that the permitted document set does not contain the answer.
- An access record of the query and what it returned.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VAULT — these apply to every VAULT skill, per `AGENTS.md` §5:**

- VAULT **extracts, compares, and flags what changed** between versions — it **never performs legal review** and never advises on the meaning or enforceability of any agreement.
- A **low-confidence extraction is flagged, never guessed** into a field as if it were certain.
- Access control is enforced **at the field level** — a role never sees a field its permissions don't cover.

**Specific to this skill:**

- **Permissions bound the search, not the results.** Searching everything and then filtering leaks through the shape of the answer — "there are no documents matching that" and "there are documents you cannot see" must be indistinguishable to the asker, and only a bounded search makes them so.
- **A citation is subject to the same field-level redaction as the document.** A cited excerpt that quotes a field the asker's role cannot see defeats `role-level-redactor` through the one channel that looks like diligence rather than access.
- **"The documents do not say" is a complete answer.** VAULT never bridges a gap in the file with an inference, an industry norm, or a likely value.
- **An answer never interprets.** Reporting what a clause states is retrieval; saying what it means, whether it is enforceable, or what it obliges anyone to do is legal review, which `legal-review-interlock` forbids without exception.
- **Every answer names the version it came from.** A confident answer from a superseded document is how a stale term reaches a borrower.
- **Every query is recorded**, including the ones that returned nothing. Query patterns are themselves an access signal WARDEN's monitoring depends on.

## Measured on

Retrieval time · answers delivered with a verifiable citation (target 100%) · redaction bypasses via citation (target zero)
