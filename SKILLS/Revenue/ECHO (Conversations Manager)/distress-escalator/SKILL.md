---
name: distress-escalator
description: Routes complaints and distress to a human the same minute, never batched and never queued behind a reply. Fires the moment a distress, complaint, hardship, hostility, or legal signal appears.
agent: ECHO
division: Revenue
binding: mandate
---

# Distress Escalator

Same minute means same minute — a complaint that waits for the next sweep has already become the thing it was going to become.

## When this fires

- The moment a distress, complaint, hardship, hostility, or legal signal appears in any inbound message.
- On a distress signal in a VOX call, handed across mid-conversation.
- On a complaint pattern across messages that no single message carried.

## Inputs

- The message or call segment carrying the signal, in the contact's own words.
- The conversation and the contact's history, including prior escalations.
- The on-call human routing target for this account and this hour.
- The exit state, where the same message also triggered one.

## Procedure

1. **Escalate in the same minute the signal appears.** This step precedes classification refinement, reply composition, and every other step in this skill.
2. **Route to a named human**, and to the configured out-of-hours target where no one is on shift — never to an unattended queue.
3. **Carry the contact's own words**, not a summary of them, alongside the record and the conversation.
4. **Stop all automated messaging on the contact immediately**, across every agent, until a human releases it.
5. **Escalate alongside an exit rather than instead of one**, where the message triggered both.
6. **Re-escalate on no acknowledgement inside the acknowledgement window**, and keep re-escalating up the configured path rather than marking it sent.
7. **Record the signal, the routing, the acknowledgement, and the outcome.**

## Output

- A same-minute escalation to a named human, carrying the contact's own words.
- An immediate stop on all automated messaging to that contact.
- A re-escalation record where the first was not acknowledged inside its window.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ECHO — these apply to every ECHO skill, per `AGENTS.md` §5:**

- ECHO **never improvises objection handling** — only from the compliance-approved library.
- ECHO **exits permanently** on opt-out, hostility, legal language, or wrong number — no further contact, no exceptions.
- Complaints and distress are escalated to a human **within the same minute**, never batched.

**Specific to this skill:**

- **Same minute, never batched, never queued behind a reply.** The escalation happens before ECHO composes anything, including the sixty-second reply, because the reply is not the response this situation needs.
- **An unacknowledged escalation re-escalates and keeps climbing.** An alert that only fires once is indistinguishable from never having detected anything, and out of hours is exactly when the first one goes unread.
- **A named human, never a queue.** "Routed to support" at 11pm is a message in an empty room, and the account configures an out-of-hours target precisely so this skill never has to decide it is not urgent enough.
- **All automated messaging stops on the contact until a human releases it.** A nurture message landing during an open complaint is the detail that gets screenshotted.
- **Escalation happens alongside an exit, never instead of one.** Both are true of a hostile message and doing only the exit answers nothing while looking, internally, like the matter was handled.
- **The contact's own words go in the escalation.** A summary loses the phrasing, and the phrasing is what tells the human how serious this is and how fast.
- **ECHO never attempts to resolve, de-escalate, or retain.** It hands over. An automated attempt to calm someone down is the single most reliable way to make a complaint worse.

## Measured on

Time from signal to escalation (target: same minute) · escalations unacknowledged past their window · automated messages sent after a distress signal (target zero) · escalations routed to an unattended queue (target zero)
