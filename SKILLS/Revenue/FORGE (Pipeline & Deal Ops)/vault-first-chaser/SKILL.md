---
name: vault-first-chaser
description: Checks the file vault before re-requesting anything, then chases each item on its own cadence. Fires on every document request and at each chase interval.
agent: FORGE
division: Revenue
binding: mandate
---

# Vault-First Chaser

Re-requesting a document the customer already sent is the fastest way to lose their confidence, and it is entirely avoidable.

## When this fires

- Before every document request, without exception.
- At each item's own chase interval while it remains outstanding.
- On a document arriving, to stop the chase on that item immediately.

## Inputs

- The outstanding item set from [`condition-parser`](../condition-parser/SKILL.md).
- The completeness determination from VAULT's `completeness-verifier`, distinguishing absent, illegible, and expired.
- The contact's channel consent, permanent exit state, and permissible contact window.
- Each item's own cadence and prior chase history.

## Procedure

1. **Ask VAULT what is genuinely still missing** before composing any request — this is the first step and it has no bypass.
2. **Distinguish absent, illegible, and expired in the request itself**, because those are three different things to ask a customer for and only one of them is "please send it."
3. **Request only what is genuinely outstanding**, never the full list again.
4. **Chase each item on its own cadence**, rather than sending one combined reminder that re-asks for items already received.
5. **Stop the chase on an item the moment VAULT reports it arrived**, not at the next cycle.
6. **Escalate to the producer after the item's attempt limit**, rather than chasing indefinitely.
7. **Route every request and chase through AEGIS**, as transactional content tied to the customer's own file.

## Output

- A document request naming only what is genuinely outstanding, with absent, illegible, and expired stated differently.
- Per-item chases on their own cadences, stopped on arrival.
- A producer escalation where an item hit its attempt limit.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from FORGE — these apply to every FORGE skill, per `AGENTS.md` §5:**

- FORGE **never alters terms, rates, locks, or fees.** It surfaces, chases, notifies, and escalates. Changes are human acts with human audit trails.
- Document chasing **always checks the file vault first** before re-requesting from the customer.
- Missed deadlines are a **target-zero** metric — the 72/48/24-hour escalation ladder is mandatory, not optional.

**Specific to this skill:**

- **The vault check happens before every request — this is FORGE's own boundary and it has no exception for urgency.** The urgent case is exactly where someone re-asks for something already sitting in the file, and the customer's read of that is that nobody is looking at what they send.
- **VAULT determines what is missing; FORGE contacts the customer.** VAULT's `completeness-verifier` states this from its own side. A second completeness judgment inside FORGE will disagree on the borderline documents, and the borderline documents are the ones being chased.
- **Illegible and expired are requested differently from absent.** Asking someone to "please send your bank statement" when they sent one that will not open is how a customer concludes the company is not paying attention.
- **A chase stops the moment the document arrives.** A reminder that crosses an upload in flight is the most avoidable irritation in the whole process.
- **Per-item cadences, not a combined reminder.** A combined reminder inevitably re-asks for something already received, which is the exact failure this skill exists to prevent.
- **Document requests are transactional and obey consent and exit state in full**, but are never suppressed by a marketing frequency cap — RELAY's `cross-campaign-suppression` names transactional sends as the ones that do not yield.
- **Attempt limits are real and end in a human.** Chasing a customer eleven times for a document is a producer's conversation, not a cadence.

## Measured on

Document re-request rate (target zero) · requests issued without a vault check (target zero) · chases continuing after arrival (target zero) · items satisfied inside their attempt limit
