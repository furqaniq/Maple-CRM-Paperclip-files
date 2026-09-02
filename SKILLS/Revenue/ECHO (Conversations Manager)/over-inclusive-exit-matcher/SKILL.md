---
name: over-inclusive-exit-matcher
description: Matches opt-out, hostility, legal, and wrong-number phrasing deliberately over-inclusively, and exits permanently when it fires. Runs on every inbound message ahead of any reply.
agent: ECHO
division: Revenue
binding: mandate
---

# Over-Inclusive Exit Matcher

The cost of a false exit is one lost lead; the cost of a missed one is a statutory violation, and that asymmetry is the whole design.

## When this fires

- On every inbound message, ahead of any reply.
- On a wrong-number signal arriving from VOX or from a bounce.
- On a merge, where either side of the merge carried an exit — the exit survives the merge.

## Inputs

- The inbound message text, in whatever language and phrasing it arrived.
- The account's configured exit phrasing sets, per channel and per jurisdiction.
- The current consent state from AEGIS's `consent-ledger`.
- The contact's channel identifiers, so the exit lands on the right ones.

## Procedure

1. **Match deliberately over-inclusively** — informal, misspelled, partial, angry, non-English, and indirect phrasings all count, and a plausible reading of an exit is an exit.
2. **Write the exit to AEGIS's `consent-ledger` immediately and irreversibly**, before any reply is composed anywhere.
3. **Apply the exit to every channel the phrasing covers**, and to every channel where the account's rules or the contact's wording make it channel-wide.
4. **Stop every queued and scheduled send to that contact across every agent**, not only ECHO's own.
5. **Escalate in parallel where the exit was hostility, legal, or distress** — the exit stops the messages and does not address what was said.
6. **Send only the confirmation the channel's rules require**, and nothing else.
7. **Record the matched phrasing and the resulting state**, so a false positive is reviewable and the contact's own later re-opt-in is possible through a human.

## Output

- A permanent exit written to the consent ledger, scoped to the channels it covers.
- A cross-agent stop on every queued send to that contact.
- A parallel escalation where the exit carried hostility, legal language, or distress.
- A record of the matched phrasing.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ECHO — these apply to every ECHO skill, per `AGENTS.md` §5:**

- ECHO **never improvises objection handling** — only from the compliance-approved library.
- ECHO **exits permanently** on opt-out, hostility, legal language, or wrong number — no further contact, no exceptions.
- Complaints and distress are escalated to a human **within the same minute**, never batched.

**Specific to this skill:**

- **An exit is permanent and ECHO never re-initiates contact on an exited channel — no exceptions, no re-permission campaign, no "we noticed you left" message.** Only the contact can restore the channel, through a human, and ECHO is never the thing that asks them to.
- **Over-inclusive means over-inclusive.** A borderline phrase resolves to an exit. Tuning this matcher toward precision to recover leads is exactly the change that produces the violation, and it will look like an improvement in every metric that is not the one that matters.
- **The exit is written before any reply is composed, anywhere in the platform.** A reply already in flight when the exit lands is a message sent after an opt-out, and the sequence is what decides that.
- **The exit stops every agent, not just ECHO.** EMBER's nurture, RELAY's campaigns, VOX's calls, TEMPO's reminders, FORGE's chasing — a per-agent exit is not an exit, it is one channel of several going quiet.
- **An exit survives a merge, always.** SCOUT's `identity-resolver` resolves consent to the most restrictive state for this reason: a merge that inherits the other record's permissive state is the quietest possible way to undo a permanent exit.
- **Hostility, legal, and distress exits escalate in parallel.** Going silent on someone who said they are going to sue is not a response to what they said, and the silence is what gets quoted back.
- **A false positive is corrected by a human, never by the matcher reconsidering.** A matcher that can reverse its own exits has no permanent exits.

## Measured on

Exit signals missed (target zero, and any non-zero value is an incident) · messages sent after an exit (target zero) · exits lost through a merge (target zero) · false-positive rate on human review
