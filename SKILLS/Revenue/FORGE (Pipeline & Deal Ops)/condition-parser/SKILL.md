---
name: condition-parser
description: Breaks requirement and condition lists into individually tracked items with plain-English translations for the customer, without interpreting what any agreement means. Fires on every condition list arriving or changing.
agent: FORGE
division: Revenue
binding: mandate
---

# Condition Parser

A condition list is unreadable as a block and trackable as items, and the translation is what stops the customer sending the wrong document three times.

## When this fires

- On every condition or requirement list arriving on a file.
- On any change to an existing list — items added, satisfied, or withdrawn.
- On a customer asking what a condition means.

## Inputs

- The condition list as issued, and its source document.
- Extraction from VAULT's `field-extractor`, with low-confidence items marked.
- The file's existing tracked items, for reconciliation.
- The account's approved plain-language phrasing set.

## Procedure

1. **Split the list into individually tracked items**, each with what is required and who can satisfy it.
2. **Translate each item into plain language from the approved phrasing set**, describing what to provide rather than what the condition means.
3. **Mark any item VAULT extracted at low confidence as unconfirmed**, and verify it with a human before it reaches the customer.
4. **Reconcile against items already tracked**, so a reissued list does not duplicate the file's open items.
5. **Route any request to interpret an agreement to VAULT's `legal-review-interlock` and to a human** — this skill describes what is being asked for, never what a document means.
6. **Track each item's own state and cadence**, and hand chasing to [`vault-first-chaser`](../vault-first-chaser/SKILL.md).
7. **Surface an item nobody can satisfy** rather than tracking it indefinitely.

## Output

- Individually tracked condition items, each with an owner and a plain-language description of what to provide.
- An unconfirmed marker on any low-confidence extraction, held from the customer until verified.
- A reconciliation result against previously tracked items.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from FORGE — these apply to every FORGE skill, per `AGENTS.md` §5:**

- FORGE **never alters terms, rates, locks, or fees.** It surfaces, chases, notifies, and escalates. Changes are human acts with human audit trails.
- Document chasing **always checks the file vault first** before re-requesting from the customer.
- Missed deadlines are a **target-zero** metric — the 72/48/24-hour escalation ladder is mandatory, not optional.

**Specific to this skill:**

- **Translation says what to provide; it never says what a condition means.** "Send the last two months of statements for the account ending 4412" is a translation. "This means they're worried about your reserves" is an interpretation, and interpretation of an agreement belongs to VAULT's legal-review boundary and to a licensed human — see [`terms-interlock`](../terms-interlock/SKILL.md).
- **A low-confidence extraction never reaches the customer unverified.** VAULT flags rather than guesses precisely so that a misread condition does not become a document request the customer cannot fulfil.
- **Items are reconciled, never duplicated.** A reissued list that recreates every open item is how a customer gets asked for the same document a second time, which is the metric this agent is measured on.
- **FORGE never adds, removes, or softens a condition.** The list is what was issued; tracking it is FORGE's job and editing it is not.
- **An unsatisfiable item is surfaced to a human, not tracked forever.** A condition nobody can meet is a file that will stall silently until its deadline.
- **Plain language comes from the approved phrasing set.** An improvised explanation of a condition is unreviewed customer-facing content on the most sensitive part of the file.
- **Nothing in a translation implies an outcome.** "Once you send this you'll be approved" is an eligibility statement attached to a document request.

## Measured on

Conditions tracked as individual items · duplicate requests from reissued lists (target zero) · unverified low-confidence items reaching the customer (target zero) · items satisfied on first request
