---
name: neglect-surfacer
description: Names the specific stalled file, unanswered reply, cooling lead, or quiet partner rather than issuing a generic nudge, with thresholds relative to each item's own baseline. Fires on the scheduled sweep and whenever an item crosses its own staleness threshold.
agent: SAGE
division: Command
binding: mandate
---

# Neglect Surfacer

Not twelve stale leads — this file, this person, this long, and what to do about it.

## When this fires

- On the scheduled sweep.
- Whenever an individual item crosses its own staleness threshold.
- Into the daily brief and the collapsed feed, where the item warrants it.

## Inputs

- Pipeline state and time-in-stage against that stage's normal duration.
- Unanswered inbound messages across every channel.
- Last-touch dates per contact, against that contact's own cadence.
- Partner and referral-source activity against their own baseline.
- Commitments made and not yet kept.

## Procedure

1. **Sweep** for files stalled past their stage's normal duration, inbound replies unanswered, leads cooling against their own cadence, and partners gone quiet against their own baseline.
2. **Name the specific record** — the file, the person, how long it has been, and what the next action is.
3. **Rank by what is recoverable**, not by what is oldest.
4. **Surface into the brief and the collapsed feed.**
5. **Track whether the surfaced item was acted on**, and escalate the ones that keep returning as a pattern rather than repeating the same line indefinitely.

## Output

- Named, specific neglected items with elapsed time and a next action, ranked by recoverability.
- A pattern escalation for items surfaced repeatedly and never acted on.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SAGE — these apply to every SAGE skill, per `AGENTS.md` §5:**

- SAGE reports to the **Account Owner**, never to ATLAS.
- SAGE is provisioned **one instance per human seat** — never shared across multiple people as a single instance.
- A decision the individual wants to make personally is **surfaced, not executed**, regardless of how routine it looks.

**Specific to this skill:**


- **Always the specific record, never an aggregate count.** "You have twelve stale leads" is the generic nudge this skill exists to replace.
- **Thresholds are relative to that item's own baseline**, not a global default — a partner who contacts quarterly is not quiet at six weeks.
- **An item surfaced repeatedly and ignored escalates as a pattern**, rather than being repeated identically forever until it is filtered out mentally.

## Measured on

Decisions surfaced versus missed · surfaced items acted on · reported time saved
