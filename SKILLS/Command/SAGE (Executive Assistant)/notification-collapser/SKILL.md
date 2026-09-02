---
name: notification-collapser
description: Reduces the raw notification feed to the three items that genuinely matter to this person today, re-evaluated continuously, with everything suppressed remaining retrievable. Fires continuously as the feed changes.
agent: SAGE
division: Command
binding: mandate
---

# Notification Collapser

Three items. Not three categories, not three digests — three things.

## When this fires

- Continuously, as the raw feed changes through the day.
- On the individual opening their feed.
- Immediately when an item arrives that displaces one of the current three.

## Inputs

- The full raw notification feed for this seat.
- The working-style profile.
- The current state of the individual's book.
- What this person has acted on before, and what they have consistently ignored.

## Procedure

1. **Ingest the full feed.** Nothing is filtered out before it has been evaluated.
2. **Score each item by consequence to this specific person today** — not by type, not by recency.
3. **Collapse to three items.**
4. **Suppress the rest**, keeping every one of them retrievable rather than discarded.
5. **Re-evaluate continuously.** The three change through the day, and displacement is expected.

## Output

- Three items, ranked, with the reason each one made the cut.
- A suppressed remainder, retrievable in full on request.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SAGE — these apply to every SAGE skill, per `AGENTS.md` §5:**

- SAGE reports to the **Account Owner**, never to ATLAS.
- SAGE is provisioned **one instance per human seat** — never shared across multiple people as a single instance.
- A decision the individual wants to make personally is **surfaced, not executed**, regardless of how routine it looks.

**Specific to this skill:**


- **Suppressed is not deleted.** Everything stays retrievable, because a collapse that loses information is a filter that will eventually lose the wrong thing.
- **Nothing that trips the neglect surfacer or carries a deadline is collapsed away.** Where more than three items qualify, the top three hold the collapsed view and the rest go to a standing must-see list that is live and readable at any time — never to a once-a-day artifact.
- **Anything falling due before the next brief escalates on its own.** An overflow route that resolves tomorrow morning is worse than no route at all, because it looks handled. Overflow is a place to read from now, not a queue for later.
- **Three is a ceiling that holds on a busy day.** A busy day is exactly when the collapse earns its keep. Overflow routes to the brief; it does not relax the ceiling.

## Measured on

Decisions surfaced versus missed · items acted on from the three · reported time saved
