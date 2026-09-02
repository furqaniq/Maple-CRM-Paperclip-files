---
name: completeness-verifier
description: Checks document completeness and legibility against what the file actually requires and tells FORGE what is genuinely still missing, so nothing is requested twice. Fires on every document arrival and on every stage change that alters the requirement set.
agent: VAULT
division: Operations
binding: mandate
---

# Completeness Verifier

The borrower who is asked a third time for a document they already sent stops answering. This skill exists to make sure that never happens.

## When this fires

- On every document arrival, against the record's current requirement set.
- On every stage change that adds or removes a requirement.
- On the scheduled completeness sweep across open files.
- Before FORGE issues any document request.

## Inputs

- The record's requirement set for its current stage and product.
- Every document already on the record, with version and legibility state.
- Expiry dates from [`version-retention-manager`](../version-retention-manager/SKILL.md) — a document present but expired is missing.
- The history of what has already been requested and when.

## Procedure

1. **Compare what is on file against what the stage actually requires.**
2. **Check legibility, not just presence.** An unreadable scan is a missing document.
3. **Check currency.** A document past its expiry is a missing document, and it is reported that way rather than as present.
4. **Subtract what has already been received**, including anything received through a channel other than the expected one.
5. **Report only what is genuinely still outstanding**, to FORGE, as a single current list.
6. **Never issue the request.** FORGE owns contact with the borrower; VAULT owns the determination of what is missing.
7. **Withdraw an item from the outstanding list the moment it arrives**, so an in-flight request stops rather than lands.

## Output

- A current outstanding-items list per record, delivered to FORGE, distinguishing absent, illegible, and expired.
- A withdrawal notice the moment an outstanding item arrives.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VAULT — these apply to every VAULT skill, per `AGENTS.md` §5:**

- VAULT **extracts, compares, and flags what changed** between versions — it **never performs legal review** and never advises on the meaning or enforceability of any agreement.
- A **low-confidence extraction is flagged, never guessed** into a field as if it were certain.
- Access control is enforced **at the field level** — a role never sees a field its permissions don't cover.

**Specific to this skill:**

- **VAULT determines what is missing; FORGE asks for it.** VAULT never contacts a borrower directly, and a completeness finding is not an outbound message.
- **An item is withdrawn from the outstanding list the instant it arrives.** A request already in flight against a document now on file is the specific failure this skill exists to prevent, and it is prevented by withdrawing immediately rather than at the next sweep.
- **Present-but-illegible and present-but-expired are both reported as missing**, with the reason stated. "We have it" against an unreadable scan is how a file reaches underwriting incomplete.
- **The outstanding list is the whole list.** A partial list produces a second request a day later, which costs more borrower goodwill than one complete ask.
- **A document received through an unexpected channel still counts as received.** Requiring the borrower to resend through the right door is a process convenience charged to the borrower.
- **VAULT never asserts a document is unnecessary.** It reports the requirement set it was given; changing that set is the loan file's decision, not VAULT's.

## Measured on

Documents requested twice (target zero) · file completeness at handoff · time from arrival to outstanding-list update
