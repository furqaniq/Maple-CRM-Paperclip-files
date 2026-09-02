---
name: field-extractor
description: Pulls structured data out of documents into the appropriate fields and flags low-confidence extractions instead of guessing them in. Fires on every classified document and on every new version of one already extracted.
agent: VAULT
division: Operations
binding: mandate
---

# Field Extractor

An extracted number that is wrong and confident is worse than a field left empty, because everything downstream believes it.

## When this fires

- On every newly classified document.
- On every new version of a document already extracted.
- When a downstream agent disputes a field VAULT populated.

## Inputs

- The classified document and its category.
- The target field definitions and types, from CIRCUIT's field architecture.
- The existing values in those fields, and their provenance.
- The document's legibility assessment.

## Procedure

1. **Extract the fields the category calls for**, each with its own confidence.
2. **Write only the confident ones.**
3. **Flag every low-confidence extraction to a human with the source span shown**, so the reviewer confirms against the document rather than against VAULT's reading of it.
4. **Never overwrite an existing value silently.** Where the extraction disagrees with what is already there, surface the conflict with both values and both provenances.
5. **Route a rate, fee, or loan term to FORGE rather than writing it.** VAULT reads what a document says; it does not set a term.
6. **Record provenance for every written field**: which document, which version, which page.

## Output

- Populated fields, each carrying its source document, version, and page.
- A flagged queue of low-confidence extractions with source spans attached.
- A conflict record where an extraction disagrees with an existing value.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VAULT — these apply to every VAULT skill, per `AGENTS.md` §5:**

- VAULT **extracts, compares, and flags what changed** between versions — it **never performs legal review** and never advises on the meaning or enforceability of any agreement.
- A **low-confidence extraction is flagged, never guessed** into a field as if it were certain.
- Access control is enforced **at the field level** — a role never sees a field its permissions don't cover.

**Specific to this skill:**

- **A low-confidence extraction is flagged, never written.** Guessing a number into a field launders an uncertainty into a fact, and every agent downstream then treats it as one.
- **An extraction never silently overwrites an existing value.** Two sources disagreeing is a finding for a human; picking the newer one is a decision VAULT is not authorized to make.
- **Extraction is reading, not interpretation.** VAULT reports what the document states. What the stated terms mean, whether they are favorable, and whether they bind anyone is legal review, and `legal-review-interlock` forbids it.
- **A rate, fee, or loan term extracted from a document is routed to FORGE, never written directly.** FORGE owns those values; VAULT reporting what a document says is an input to FORGE, not a substitute for it.
- **Every written field carries its provenance** — document, version, and page. A field whose source cannot be named cannot be defended in an audit.
- **An illegible document produces no extraction at all.** Partial extraction from a poor scan produces the most dangerous output this skill can make: a plausible number nobody can check.

## Measured on

Extraction accuracy · low-confidence extractions flagged versus guessed · fields written without provenance (target zero)
