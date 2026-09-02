---
name: identity-resolver
description: Collapses one human arriving four times through four channels with three phone formats into a single record, carrying the most restrictive consent and exit state forward. Fires on every normalized record and on any later merge or split request.
agent: SCOUT
division: Revenue
binding: mandate
---

# Identity Resolver

One human, one record — and a merge never gives a contact back a channel they already closed.

## When this fires

- On every record leaving [`source-normalizer`](../source-normalizer/SKILL.md).
- On a manual merge or split request from a user.
- When a later enrichment or correction reveals two records are the same person.
- On a bulk import, before any of it is written.

## Inputs

- The normalized incoming record and the existing candidate matches.
- Match evidence per candidate — exact and fuzzy, across identifiers.
- Consent state per channel for every candidate record, from AEGIS's `consent-ledger`.
- Permanent exit state from ECHO's `over-inclusive-exit-matcher`, on every candidate.
- The per-contact brief from ATLAS's `memory-brief-store`, for interaction history already known.

## Procedure

1. **Score candidate matches on identifier evidence** — email, phone, name plus address, device and form fingerprint — and treat each identifier's strength honestly rather than counting weak matches as strong ones.
2. **Merge above the confidence threshold; hold for human review below it.** A wrong merge destroys two records and is far harder to reverse than a duplicate is to catch.
3. **Carry the most restrictive consent and exit state across the merged record, always.** An opt-out, a hostility exit, a legal exit, or a wrong-number exit on *either* side applies to the merged record on that channel. This step never resolves toward the more permissive state, whichever record is older, richer, or more recently active.
4. **Preserve both source lineages** on the merged record rather than keeping the newer one — attribution and spend both read this.
5. **Keep the merge reversible**: retain what each side contributed, so a wrong merge can be split without reconstructing either record from memory.
6. **Update the per-contact brief** so no agent re-asks something the surviving record already answers.
7. **Report the duplicate to [`junk-duplicate-spend-filter`](../junk-duplicate-spend-filter/SKILL.md)** where the same person arrived twice through paid sources.

## Output

- A single resolved record with both lineages and the most restrictive consent and exit state.
- A held-for-review pair where evidence was below the merge threshold, with the specific evidence shown.
- A duplicate-arrival notice where paid spend produced the same person twice.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SCOUT — these apply to every SCOUT skill, per `AGENTS.md` §5:**

- SCOUT **never pulls, requests, infers, or stores consumer credit information**, under any circumstance.
- Position estimates are **always labeled as estimates** — never presented as fact.

**Specific to this skill:**

- **A merge resolves consent and exit state to the most restrictive value on every channel, without exception.** This is the rule that matters most in this skill. An opt-out, hostility, legal, or wrong-number exit is permanent under ECHO's boundary, and a merge that inherits the other record's permissive state hands a contact who closed a channel straight back to the agent that will use it. A quiet, correct-looking merge is the most plausible way a permanent exit gets undone, and nothing downstream would show it had happened.
- **A merge below the confidence threshold is held for a human, never resolved by preferring the richer record.** Two people who share a household, a surname, and a landline are a common case, not an edge one.
- **Both source lineages survive a merge.** Dropping the older one silently reassigns credit for the lead and quietly corrupts LEDGER's `event-stream-attribution`, which is where spend by channel and campaign is held.
- **Every merge is reversible and records what each side contributed.** An irreversible merge means a false match is a permanent data loss rather than a correction.
- **Resolution uses identifiers and interaction evidence only.** Name, ethnicity-suggestive spelling, neighborhood, and language are not match signals, and using them turns a resolver into a classifier of people.
- **No credit identifier is ever a match key** — see [`credit-data-firewall`](../credit-data-firewall/SKILL.md).

## Measured on

Duplicate rate · false merges (target zero) · exits or opt-outs lost through a merge (target zero, and any non-zero value is an incident) · held pairs resolved within their window
