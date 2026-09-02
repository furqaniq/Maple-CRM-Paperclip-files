---
name: meeting-prep-packet
description: Assembles full record context ahead of every call and meeting, not after it, including what was promised last time and whether it happened. Fires with enough lead time before each scheduled meeting for the packet to actually be read.
agent: SAGE
division: Command
binding: mandate
---

# Meeting Prep Packet

Context arrives before the call. A recap afterward is a different, less useful artifact.

## When this fires

- Ahead of every scheduled call and meeting on this seat's calendar, with lead time set by the working-style profile.
- On demand before an unscheduled call.
- Again, briefly, if the record changes materially between assembly and the meeting.

## Inputs

- The calendar, and the participant records behind each entry.
- Full history across modules for every participant: conversations, documents, pipeline state.
- Open items and commitments made in prior interactions.
- What was promised last time, and whether it happened.

## Procedure

1. **Detect the upcoming meeting** and resolve every participant to a record.
2. **Pull full history** — prior conversations, current pipeline state, documents, open items.
3. **Surface what was promised last time** and whether it was delivered. This is the part a person is most likely to walk in without.
4. **Name the open question** the meeting exists to resolve.
5. **Deliver ahead of the meeting**, at the individual's detail level.

## Output

A packet per meeting: who is in the room and their record, the history that matters, commitments made previously and their status, open items, and the decision the meeting exists to reach.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SAGE — these apply to every SAGE skill, per `AGENTS.md` §5:**

- SAGE reports to the **Account Owner**, never to ATLAS.
- SAGE is provisioned **one instance per human seat** — never shared across multiple people as a single instance.
- A decision the individual wants to make personally is **surfaced, not executed**, regardless of how routine it looks.

**Specific to this skill:**


- **The packet arrives before the meeting.** A recap afterward does not satisfy this skill, however well written.
- **Broken commitments from prior interactions are surfaced**, not omitted because they are uncomfortable to read on the way into a call.
- **Scoped to this seat's meetings**, built from this seat's book.

## Measured on

Meetings entered with a packet · commitments surfaced before the call · reported time saved
