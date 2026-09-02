---
name: auto-classifier
description: Files every upload to the correct record and category without a human choosing a folder. Fires on every document entering the platform, from any channel, and on any document whose record association is later disputed.
agent: VAULT
division: Operations
binding: mandate
---

# Auto-Classifier

Every document that enters the platform gets read and placed. The only two outcomes are a confident placement and a held document — never a confident-looking guess.

## When this fires

- On every document entering the platform, from any channel — upload, email, e-signature return, or integration.
- When a document's record association is disputed or corrected.
- When a document arrives with no identifying content at all.

## Inputs

- The document itself, and its channel of arrival.
- The contact and loan record set it might belong to.
- The document taxonomy — categories, expected types per stage.
- Identifying content within the document: names, addresses, loan numbers, dates.
- The uploading user's permissions, since a document cannot be filed to a record they cannot see.

## Procedure

1. **Read the document** and extract its identifying content.
2. **Determine the record** it belongs to and the category it falls into, with a confidence on each independently — a document can be confidently a pay stub and uncertainly this contact's.
3. **File it where both are confident.**
4. **Hold it where either is not.** A held document goes to a review queue named to a human; it is never filed to a best-guess record.
5. **Never file across records on a partial match.** A shared surname, a shared address, or a shared employer is not an identification.
6. **Record the classification decision and its confidence** so a later dispute can be traced rather than re-argued.
7. **Hand the filed document to [`field-extractor`](../field-extractor/SKILL.md) and [`completeness-verifier`](../completeness-verifier/SKILL.md).**

## Output

- A filed document with its record, category, and classification confidence recorded.
- Or a held document in a named review queue, with what was ambiguous stated.
- A classification decision record for audit.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VAULT — these apply to every VAULT skill, per `AGENTS.md` §5:**

- VAULT **extracts, compares, and flags what changed** between versions — it **never performs legal review** and never advises on the meaning or enforceability of any agreement.
- A **low-confidence extraction is flagged, never guessed** into a field as if it were certain.
- Access control is enforced **at the field level** — a role never sees a field its permissions don't cover.

**Specific to this skill:**

- **A document is never filed to a best-guess record.** Misfiling a financial document to the wrong contact is a privacy breach that looks like ordinary filing, and it is not discovered until the wrong person is shown their neighbor's income. VAULT operates at L3 precisely because this skill holds rather than guesses.
- **Record confidence and category confidence are separate judgments.** Being sure what a document is says nothing about whose it is, and a single blended score hides exactly the case that matters.
- **A partial identity match is not an identification.** A shared surname, address, employer, or phone number holds the document; it never resolves it.
- **A held document is visible and named to a human**, never left aging in a queue nobody watches. A document held forever and a document lost are the same outcome for the borrower waiting on it.
- **Classification never overrides the field-level access model.** A document is filed to a record; who can then see it, and which of its fields, is [`role-level-redactor`](../role-level-redactor/SKILL.md)'s determination and not this skill's.
- **A corrected misfiling is recorded as a correction, not a silent refile.** The exposure that already happened is the finding, and it does not disappear when the document moves.

## Measured on

Classification accuracy · documents misfiled to the wrong record (target zero) · held-document resolution time
