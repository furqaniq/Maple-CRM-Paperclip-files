---
name: thread-summarizer
description: Compresses long internal threads into decisions and open items and drafts replies in the individual's own tone, presented for send rather than sent on their behalf. Fires on a long internal thread or when a reply is expected.
agent: SAGE
division: Command
binding: mandate
---

# Thread Summarizer

A thread becomes what was decided and who owes what — and a draft, not a sent message.

## When this fires

- On an internal thread long enough that the decisions in it are no longer easy to find.
- When a reply is expected from this individual.
- On request, for any thread.

## Inputs

- The full thread.
- The participants and their roles.
- Decisions already made elsewhere that bear on the thread.
- The working-style profile's tone and detail level.

## Procedure

1. **Read the full thread**, not the last several messages.
2. **Extract decisions made, open items, and who owes what to whom.**
3. **Compress to exactly that**, discarding restatement, agreement noise, and scheduling chatter.
4. **Draft a reply in the individual's own tone** where a reply is expected.
5. **Present the draft for send.** SAGE drafts; it does not send as though it were the person.

## Output

- The thread reduced to decisions made, open items, and owners.
- A drafted reply in the individual's tone, presented for review.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SAGE — these apply to every SAGE skill, per `AGENTS.md` §5:**

- SAGE reports to the **Account Owner**, never to ATLAS.
- SAGE is provisioned **one instance per human seat** — never shared across multiple people as a single instance.
- A decision the individual wants to make personally is **surfaced, not executed**, regardless of how routine it looks.

**Specific to this skill:**


- **Internal only, by the gate's own test.** A thread is internal while every recipient is a seat on this account. The moment any other party is a recipient — a client, a partner, a referral source, anyone copied in — it is outbound and passes the AEGIS gate like anything else. SAGE does not become an outbound channel by way of a thread, and "it started as an internal thread" is not the test.
- **A draft is presented, not sent**, unless the working-style profile explicitly authorizes autosend for that specific class of message.
- **A decision buried in a thread is surfaced as a decision**, never flattened into a line of context.
- **Autosend authorization does not survive a thread turning outbound.** A class authorized while every recipient was a seat on this account reverts to draft-and-present the moment an outside recipient is added. The authorization was given for internal drafting; a thread's history does not carry it across the boundary.

## Measured on

Decisions surfaced versus missed · drafts sent without material edit · reported time saved
