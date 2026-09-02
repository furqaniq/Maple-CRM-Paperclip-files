---
name: rolling-calendar
description: Maintains a forward content calendar balanced across pillars and flags gaps before they become dead weeks. Fires on the planning cycle, when the horizon shortens, and whenever a scheduled item is withdrawn.
agent: BEACON
division: Marketing
binding: mandate
---

# Rolling Calendar

Reporting a dead week after it happened is not this skill.

## When this fires

- On the scheduled planning cycle.
- When the forward horizon drops below its target coverage.
- When a pillar falls out of balance.
- When a scheduled item is withdrawn — a CANVAS expiry, an AEGIS block, a QUILL template retirement, a licensing change.

## Inputs

- The pillar definitions and their target balance.
- Scheduled and published items across every connected platform.
- Per-platform cadence targets.
- Library availability from CANVAS, and copy-pipeline state from QUILL.
- **RELAY's campaign calendar**, so a social push and an email push do not land on the same contacts on the same day.
- Seasonal and market events with real lead times.

## Procedure

1. **Measure the forward horizon in days of covered slots**, not in item count — ten posts clustered in one week is not three weeks of coverage.
2. **Check pillar balance.** A calendar full of one pillar is a gap, not a full calendar.
3. **Flag gaps ahead of the dead week**, at the point they are still fillable, naming the pillar and the lead time needed to fill it.
4. **Coordinate against RELAY's campaign calendar** so social and email pushes do not collide on the same audience in the same window.
5. **Reserve slots rather than assume assets.** A slot with no cleared asset behind it is a reservation, not coverage.
6. **On any withdrawal, reopen the slot visibly and re-flag**, rather than leaving the calendar showing coverage it no longer has.

## Output

A rolling calendar stating days of real coverage, pillar balance against target, reserved-but-unfilled slots distinguished from cleared ones, collisions with RELAY's campaigns, and gaps flagged with the lead time each needs.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from BEACON — these apply to every BEACON skill, per `AGENTS.md` §5:**

- Genuine leads surfacing in the social inbox are **routed into the CRM**, never left to die inside the platform.
- Anything in mentions, reviews, or competitor activity that needs a human is escalated **within the hour**.
- Reporting is always in **leads and pipeline**, never vanity metrics.

**Specific to this skill:**

- **A gap is flagged before it becomes a dead week**, with enough lead time to actually fill it. A gap reported at the point it is unfixable is a record, not a warning.
- **A slot with no cleared asset is not coverage.** Planned is not scheduled, and a calendar that counts intentions is a calendar that reports weeks it did not have.
- **The calendar never fills a gap by lowering the bar** — no republishing an expired asset, no cross-posting an unadapted one, and nothing scheduled that has not cleared every gate that applies to it. An unfilled slot is reported as unfilled.
- **A withdrawn item reopens its slot visibly.** The calendar never continues to show coverage that has been pulled, because the whole point of the horizon is that someone can trust it.

## Measured on

Publishing consistency · engagement rate
