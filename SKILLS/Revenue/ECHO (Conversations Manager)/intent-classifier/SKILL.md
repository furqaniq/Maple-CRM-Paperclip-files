---
name: intent-classifier
description: Assigns exactly one of ten intents to every inbound message — hot, question, objection, reschedule, wrong number, opt-out, hostile, legal, auto-reply, unclear — under a fixed precedence that puts the exit and safety intents first.
agent: ECHO
division: Revenue
binding: mandate
---

# Intent Classifier

Ten intents, one per message, and a precedence order that decides what happens when a message is genuinely two of them at once.

## When this fires

- On every inbound message, before any reply is composed.
- On a re-classification when a later message changes what an earlier one meant.

## Inputs

- The inbound message and the conversation so far.
- The contact's history, including prior exits, complaints, and escalations.
- The classifier's precedence order and confidence thresholds.
- The account's configured phrasing sets for the exit intents.

## Procedure

1. **Evaluate the exit and safety intents first** — opt-out, hostile, legal, wrong number, and distress — before considering whether the message is also a question, an objection, or hot.
2. **Apply the precedence order rather than the highest score.** A message that is both an objection and an opt-out is an opt-out; a message that is both hot and legal is legal.
3. **Assign exactly one intent** and record the alternatives it outranked, so the precedence decision is visible rather than lost.
4. **Escalate on the safety intents in addition to classifying them** — a hostile or legal message is routed to [`distress-escalator`](../distress-escalator/SKILL.md) as well as exited, because an exit stops the messaging and does not answer the complaint.
5. **Classify as unclear rather than guessing.** Unclear routes to a human; a wrong confident intent routes to the wrong response entirely.
6. **Treat auto-reply as a real intent**, so an out-of-office is never read as engagement and never restarts a cadence.
7. **Write the intent, its confidence, and what it outranked to the record.**

## Output

- One intent per message, with confidence and the intents it outranked.
- A parallel escalation where a safety intent was present.
- An unclear classification routed to a human rather than answered.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ECHO — these apply to every ECHO skill, per `AGENTS.md` §5:**

- ECHO **never improvises objection handling** — only from the compliance-approved library.
- ECHO **exits permanently** on opt-out, hostility, legal language, or wrong number — no further contact, no exceptions.
- Complaints and distress are escalated to a human **within the same minute**, never batched.

**Specific to this skill:**

- **Exactly one intent per message, chosen by precedence, never by score.** The whole value of a single-intent model is that the response is unambiguous, and the whole risk is in the messages that are honestly two things — which is why the order is fixed rather than computed.
- **Exit and safety intents outrank every commercial intent, always.** A message that reads as both hot and hostile is hostile. Ranking the commercial reading higher is precisely the failure that turns an angry customer into a complaint on the record.
- **Classification is deliberately over-inclusive on the exit intents**, per [`over-inclusive-exit-matcher`](../over-inclusive-exit-matcher/SKILL.md). The cost of a false exit is a lost lead; the cost of a missed one is a statutory violation, and the two are not comparable.
- **Unclear is a real classification, not a fallback to the most likely intent.** A confident wrong intent is worse than an admitted uncertainty, because only one of them gets reviewed.
- **A safety intent escalates as well as classifies.** Exiting a hostile contact stops the messages and leaves the complaint unanswered; both need to happen, and only one of them is automatic.
- **Intent is read from the message, never from the person.** Language, dialect, name, and phrasing style are not hostility signals, and a classifier that learns them routes people to the exit path on a protected characteristic.
- **An auto-reply never counts as engagement.** Reading an out-of-office as interest restarts sequences, inflates every engagement metric, and eventually calls somebody on holiday.

## Measured on

Classification accuracy · exit intents missed (target zero) · safety intents classified without a parallel escalation (target zero) · unclear rate and its human-review outcome
