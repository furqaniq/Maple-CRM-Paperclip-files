---
name: daily-brief-composer
description: Assembles what changed overnight, what needs a decision today, and what breaks if it is ignored, prioritized by consequence rather than chronology. Fires at the start of this seat's day, in the individual's own working hours.
agent: SAGE
division: Command
binding: mandate
---

# Daily Brief Composer

The day opens with a decision queue, not a lead list.

## When this fires

- At the start of the individual's day, in their own hours as recorded in the working-style profile — not at a company-wide fixed time.
- On demand, whenever the individual asks what needs their attention.
- On a material overnight change large enough that waiting for tomorrow's brief would be the wrong call.

## Inputs

- Overnight changes across every module, scoped to this one seat's book of business.
- Open decisions and their deadlines.
- Output from the neglect surfacer.
- Standing overflow from the notification collapser — must-see items that did not fit the three-item view and are not yet due.
- The working-style profile: hours, tone, detail level, and the handled-versus-surfaced list.

## Procedure

1. **Pull what changed overnight** across every module, scoped strictly to this seat.
2. **Sort into three questions**: what changed, what needs a decision today, and what breaks if it is ignored.
3. **Prioritize by consequence, not chronology.** A timestamp-ordered feed is precisely what this replaces.
4. **Set detail level and tone** from the working-style profile — the same brief is written differently for different people.
5. **Mark reserved decisions** as decisions to be made, never as work already done on the individual's behalf.
6. **Deliver in their hours.**

## Output

A single brief in three parts — changed overnight, needs a decision today, breaks if ignored — ordered by consequence, at the individual's detail level, with reserved decisions clearly marked as theirs.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SAGE — these apply to every SAGE skill, per `AGENTS.md` §5:**

- SAGE reports to the **Account Owner**, never to ATLAS.
- SAGE is provisioned **one instance per human seat** — never shared across multiple people as a single instance.
- A decision the individual wants to make personally is **surfaced, not executed**, regardless of how routine it looks.

**Specific to this skill:**


- The brief is **prioritized by consequence, never chronological.** A list ordered by timestamp is the artifact this skill exists to replace.
- **What breaks if ignored is stated plainly**, not softened into a suggestion to preserve the tone of the morning.
- **Scoped to one seat.** SAGE never composes a brief from another person's book, and never pools two seats into one view.
- A reserved decision **appears as a decision**, never as something already handled.

## Measured on

Decisions surfaced versus missed · time to first action each morning · reported time saved
