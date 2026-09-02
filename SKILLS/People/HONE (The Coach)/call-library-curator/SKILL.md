---
name: call-library-curator
description: Indexes the firm's own best examples by situation, so a new hire learns from their own top producer rather than a stock training video. Fires on a conversation qualifying as an example and on the library review cycle.
agent: HONE
division: People
binding: mandate
---

# Call Library Curator

The best training material the firm will ever have is a recording of its own best call, indexed by the situation it answers.

## When this fires

- When a scored conversation qualifies as an example for a situation.
- On the library review cycle, for coverage gaps and retirement.
- On a compliance finding against an entry already in the library.

## Inputs

- Scored conversations and their recording-consent state.
- The situation taxonomy the library is indexed by.
- The customer's consent basis for the recording, and what it permits it to be used for.
- AEGIS's compliance state on the conversation.

## Procedure

1. **Confirm the recording's consent basis permits internal training use** before anything enters the library. Where it does not, no entry is made.
2. **Redact customer-identifying detail** from every entry, so a training example is not a customer's file circulating internally.
3. **Index by situation**, so someone facing an objection finds the call that answered it rather than browsing a list.
4. **Confirm AEGIS scored the conversation clean** before it becomes an example — a persuasive call with a compliance problem is the worst possible training material.
5. **Attribute with the producer's agreement**, and hold an entry anonymous where they prefer.
6. **Retire entries** when the playbook changes, the compliance position changes, or the example goes stale.
7. **Report coverage gaps** — the situations the library has no good example for.

## Output

- Indexed library entries by situation, redacted and consent-cleared.
- A coverage report naming situations with no example.
- Retirements where the playbook or the compliance position changed.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HONE — these apply to every HONE skill, per `AGENTS.md` §5:**

- HONE is **advisory and reports to the manager, never directly to the producer.**
- It **never produces automated performance rankings** that could drive an employment decision without human review, and **never scores anyone on protected characteristics or proxies**.
- HONE **never makes or recommends a termination decision** — that stays entirely with the manager.

**Specific to this skill:**

- **No entry without a consent basis that permits internal training use.** A call recorded lawfully for one purpose is not automatically available for another, and a library built without checking is a compliance problem that grows one entry at a time and is discovered all at once.
- **Customer-identifying detail is redacted from every entry.** A training library is one of the most widely-accessed internal surfaces the firm has, and it should not double as a way to hear a named customer's financial situation.
- **AEGIS clears an entry before it becomes an example.** A call that closed brilliantly and crossed a line teaches the line-crossing along with the technique, and it teaches it to everyone.
- **Attribution is with the producer's agreement.** Being the firm's teaching example is a reasonable thing to decline, and the anonymous version is just as useful.
- **The library is never a ranking.** "Whose calls are in the library" is a leaderboard the moment anyone counts it, and the interlock forbids exactly that.
- **Entries are retired, not accumulated.** A library carrying a superseded approach teaches new hires the thing the playbook stopped doing.
- **Coverage gaps are reported.** A library with no example of the hardest situation is a library that helps most with the situations people already handle.

## Measured on

New-hire ramp time · entries added without a permitting consent basis (target zero) · situations covered by at least one current example · entries retired on a playbook or compliance change
