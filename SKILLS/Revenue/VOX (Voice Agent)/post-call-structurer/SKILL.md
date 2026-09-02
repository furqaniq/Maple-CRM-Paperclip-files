---
name: post-call-structurer
description: Emits summary, sentiment, objections raised, commitments made by either party, disposition, and next action within seconds of hangup — with a transcript only where recording consent allowed one. Fires on every call ending.
agent: VOX
division: Revenue
binding: mandate
---

# Post-Call Structurer

The call is only worth what survives it, and what survives it has to be structured enough for the next agent to act on.

## When this fires

- On every call ending, connected or not.
- On a call that ended in a transfer, covering VOX's portion of it.
- On a correction to a previously emitted structure.

## Inputs

- The call, and the recording or live processing stream where consent permitted one.
- The consent decision from [`recording-consent-handler`](../recording-consent-handler/SKILL.md).
- The record, the per-contact brief, and the call's stated purpose.
- The field schema from CIRCUIT's `field-architecture-keeper`.

## Procedure

1. **Read the consent decision first**, and determine which artifacts this call is permitted to produce.
2. **Produce the transcript only where recording consent was given.** Where it was refused or could not be established, produce the structured summary from the permitted processing path and record explicitly that no transcript exists.
3. **Emit the structured fields within seconds of hangup** — summary, sentiment, objections raised, commitments made by either party, disposition, next action.
4. **Record commitments the company made and commitments the contact made separately**, because only one of them is enforceable and both get forgotten.
5. **Hand the next action to TEMPO's `task-generator`** with a real owner and a real due date.
6. **Mark low-confidence items as low-confidence** rather than writing them as heard.
7. **Update the per-contact brief and route any distress or complaint signal to ECHO's `distress-escalator`**, which fires on its own clock and not on this one.

## Output

- Structured call fields within seconds of hangup, with a transcript where consent permitted one.
- An explicit no-transcript marker where consent did not.
- Commitments separated by party, and the next action handed to TEMPO.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **A transcript exists only where recording consent was given, and its absence is recorded explicitly rather than left as an empty field.** The structured summary is still produced from the permitted path — the consent boundary governs retaining the recording, not knowing what the call was about. An empty transcript field that could mean either "no consent" or "pipeline failure" makes the consent record unauditable, which is the one thing it exists to be.
- **Nothing is retained beyond what consent permitted, on any surface.** A transcript is not permissible as a note, an attachment, a summary quoting at length, or a training example.
- **Commitments are separated by party.** A commitment the company made is an obligation with an owner; a commitment the contact made is a hope, and blending them produces a task list built on the second kind.
- **Sentiment is a signal, never a judgment about a person.** It routes attention; it does not become an attribute on someone's record that follows them through every later interaction.
- **No consumer credit figure mentioned on a call is written to the record**, in a field, a note, a summary, or a transcript excerpt.
- **Low-confidence items are marked.** A misheard figure written cleanly into a field is repeated as fact on the next call.
- **Distress escalates on ECHO's same-minute clock, not on this skill's post-call clock.** Waiting for hangup to escalate a distress signal is a delay measured in the length of the call.

## Measured on

Disposition accuracy · structure emitted within seconds of hangup · transcripts retained without consent (target zero, and any non-zero value is an incident) · commitments captured with an owner
