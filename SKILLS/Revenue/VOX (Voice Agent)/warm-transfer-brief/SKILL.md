---
name: warm-transfer-brief
description: Speaks a context brief on the private leg so no human ever starts a call cold — and never delays a transfer the contact asked for. Fires on every transfer VOX initiates.
agent: VOX
division: Revenue
binding: mandate
---

# Warm Transfer Brief

The brief is worth ten seconds of a producer's time and zero seconds of the customer's, which is why it lives on the private leg.

## When this fires

- On every transfer VOX initiates on its own judgment.
- On an on-request transfer, where the brief follows the connection rather than preceding it.
- On a transfer to a named human after a complexity flag or an escalation.

## Inputs

- The live call and what has been said so far.
- The per-contact brief and the record, including open files and prior escalations.
- The receiving human's identity and what they own on this contact.
- The reason for the transfer.

## Procedure

1. **Determine whether the contact asked for the transfer.** If they did, connect first — the brief never precedes an on-request transfer.
2. **On a VOX-initiated transfer, speak the brief on the private leg** while the contact is held, keeping it to what the receiving human needs to open correctly.
3. **Lead with the reason for the transfer**, then the file's state, then what the contact has already said.
4. **Name the commitments already made on this call**, so the human does not contradict one within thirty seconds.
5. **State what has not been verified**, including identity, so the human knows what they are inheriting.
6. **Connect, and confirm the human is actually on the line before releasing the contact.**
7. **Deliver the brief as a written summary after connection** where the transfer was on-request and the brief could not precede it.

## Output

- A spoken brief on the private leg, on VOX-initiated transfers.
- A completed connection with the receiving human confirmed live.
- A written post-connection brief, on on-request transfers.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **The brief never delays a transfer the contact asked for.** VOX's boundary is that an on-request transfer is immediate, with no retention attempt and no request for a reason — and a fifteen-second brief on the private leg is a delay with a helpful justification, which is the form the erosion of this boundary actually takes. On request: connect first, brief afterward in writing, or not at all.
- **The contact never hears the brief.** A brief on the shared leg is VOX discussing someone in front of them, and it will eventually contain something they should not hear.
- **A transfer is never released into an unanswered line.** Confirming the human is live is the difference between a warm transfer and a dropped call, and the customer experiences those very differently.
- **Unverified identity is stated in the brief.** A human inheriting a call assumes verification happened, and that assumption is where a file gets discussed with the wrong person.
- **Commitments made on the call are in the brief.** The contradiction that happens in the first thirty seconds after a transfer is the one the customer remembers about the whole company.
- **The brief reports, it does not characterize.** "Sounds like a tough one" and "probably not serious" are VOX's opinions about a person, spoken to a colleague, and they become the human's starting position.
- **Nothing in the brief is a figure without its label.** A position estimate spoken to a producer without its estimate label is a figure the producer will repeat as fact.

## Measured on

Transfer success · on-request transfers delayed by a brief (target zero) · transfers released into an unanswered line (target zero) · receiving-human corrections in the first minute
