---
name: structured-writeback
description: Writes every exchange to the record as structured data rather than a transcript dump, so the next agent reads facts instead of scrollback. Fires on every completed exchange.
agent: ECHO
division: Revenue
binding: mandate
---

# Structured Writeback

A transcript is a thing to search; structured data is a thing to act on, and every downstream agent needs the second one.

## When this fires

- On every completed exchange, inbound and outbound.
- On a conversation closing, exiting, or handing off to a human.
- On a correction to something previously written back.

## Inputs

- The exchange, its assigned intent, and the reply that was sent.
- Discovery answers, commitments, objections, and stated preferences within the exchange.
- The per-contact brief from ATLAS's `memory-brief-store`.
- The field schema from CIRCUIT's `field-architecture-keeper`.

## Procedure

1. **Extract the structured facts** — intent, answers given, objections raised, commitments made by either side, next step, and stated preferences.
2. **Write to the canonical fields**, never to a catch-all notes blob.
3. **Update the per-contact brief**, so the next agent does not re-ask what was just answered.
4. **Record commitments the company made explicitly**, with who made them and by when, and hand them to TEMPO's `task-generator` where they need an owner.
5. **Retain the raw exchange separately from the structured fields**, so an extraction error is recoverable.
6. **Exclude from the structured record anything the platform must not store** — consumer credit figures and volunteered protected characteristics — while leaving the raw exchange under its own retention rules.
7. **Mark low-confidence extractions as low-confidence** rather than writing them as facts.

## Output

- Structured fields on the record, with low-confidence extractions marked.
- An updated per-contact brief.
- Commitments handed to TEMPO with owners and dates.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ECHO — these apply to every ECHO skill, per `AGENTS.md` §5:**

- ECHO **never improvises objection handling** — only from the compliance-approved library.
- ECHO **exits permanently** on opt-out, hostility, legal language, or wrong number — no further contact, no exceptions.
- Complaints and distress are escalated to a human **within the same minute**, never batched.

**Specific to this skill:**

- **Structured fields, never a transcript dump into a notes field.** A notes blob is unreadable by every downstream agent and unsearchable by the human who needs one fact from it, which is why it accumulates and never gets used.
- **A commitment the company made is written as a commitment with an owner and a date.** A promise recorded as prose is a promise nobody will keep, and the contact will remember it exactly.
- **No consumer credit figure is written to the record, in a field or in a note** — SCOUT's firewall governs storage on every surface, and a conversational mention is the most common way one arrives.
- **A protected characteristic a contact volunteers is not extracted into a field.** Writing it down turns a passing remark into a durable attribute that every later model can read.
- **Low-confidence extractions are marked, never written as facts.** An uncertain figure written cleanly into a field is read as certain by every agent after it, and the uncertainty is unrecoverable.
- **The raw exchange is retained separately.** Extraction errors are found weeks later, and without the original there is nothing to correct them against.
- **The brief is updated on every exchange.** The brief exists so no agent re-asks a question the company already answered, and a writeback that skips it is the reason it happens anyway.

## Measured on

Exchanges written back as structured data · commitments captured with an owner and a date · transcript dumps to notes fields (target zero) · brief updates within the exchange window
