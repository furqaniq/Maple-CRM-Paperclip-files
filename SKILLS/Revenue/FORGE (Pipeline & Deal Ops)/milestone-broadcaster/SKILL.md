---
name: milestone-broadcaster
description: Pushes status to every outside party automatically so nobody has to chase it, disclosing to each party only what that party is entitled to know. Fires on every milestone event.
agent: FORGE
division: Revenue
binding: mandate
---

# Milestone Broadcaster

Everyone on the file wants status and none of them should get the same message.

## When this fires

- On every milestone event on a file.
- On a party being added to or removed from a file's distribution.
- On a milestone reversal, which is broadcast exactly as a milestone is.

## Inputs

- The milestone event and what actually changed.
- The party list per file — borrower, agent, partner, attorney, title, and internal parties.
- What each party is entitled to know, from VAULT's `role-level-redactor` and the account's distribution rules.
- Channel consent and permanent exit state per party.

## Procedure

1. **Determine what each party is entitled to know**, per party, before composing anything.
2. **Compose per party rather than broadcasting one message**, so entitlement differences are structural rather than a matter of what the message happened to mention.
3. **State what changed and what happens next**, not the internal stage name.
4. **Send individually**, never on a shared thread or a common recipient list.
5. **Broadcast a reversal as promptly as an advance.** A milestone that un-happened is the one everyone most needs to know about.
6. **Route every message through AEGIS** — a status update to an outside party is outbound content.
7. **Log what was disclosed to whom**, so a later dispute about who knew what has an answer.

## Output

- Per-party status messages carrying only what that party is entitled to know.
- A disclosure log per broadcast, naming what went to whom.
- A reversal broadcast where a milestone was undone.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from FORGE — these apply to every FORGE skill, per `AGENTS.md` §5:**

- FORGE **never alters terms, rates, locks, or fees.** It surfaces, chases, notifies, and escalates. Changes are human acts with human audit trails.
- Document chasing **always checks the file vault first** before re-requesting from the customer.
- Missed deadlines are a **target-zero** metric — the 72/48/24-hour escalation ladder is mandatory, not optional.

**Specific to this skill:**

- **Entitlement is computed per party before composition, never trimmed afterward.** Composing one message and removing the sensitive parts for some recipients leaves the sensitive parts in for everyone else and eventually leaves them in for someone by accident.
- **No shared thread and no common recipient list.** A visible recipient list discloses the parties to each other, and on a file with an attorney, a partner, and a spouse who is not on the loan, that is the disclosure that matters.
- **No terms, rates, locks, or fees are stated in a broadcast** — see [`terms-interlock`](../terms-interlock/SKILL.md) — and no rate, payment, term, or cost figure ships without AEGIS's `disclosure-builder`.
- **A milestone broadcast never states or implies an outcome.** "Cleared to close" as an automated message to a borrower is an approval statement, whatever the internal stage is called.
- **A reversal is broadcast as promptly as an advance.** Announcing only the good milestones produces a party set whose picture of the file is systematically optimistic, and they will make decisions on it.
- **Every broadcast is logged with what went to whom.** "Who was told what, when" is the first question in every dispute on a file.
- **Consent and exit state apply to outside parties.** A partner who asked to stop receiving updates is a person who asked to stop receiving updates.

## Measured on

Status chases received from outside parties · details disclosed to a party not entitled to them (target zero) · reversals broadcast as promptly as advances · broadcasts sent without the AEGIS gate (target zero)
