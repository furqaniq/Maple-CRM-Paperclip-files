---
name: availability-engine
description: Maintains real-time availability across the team, honoring working hours, buffers, travel time, and time zones, and serves it as the platform's single source of truth. Runs continuously and answers every availability question in the platform.
agent: TEMPO
division: Revenue
binding: mandate
---

# Availability Engine

One definition of when someone is free, held in one place, or every agent eventually books against a different one.

## When this fires

- Continuously, against every connected calendar.
- On every availability query from another agent or skill.
- On a working-hours, territory, or org-structure change.
- On a hold request, a hold release, and a hold expiry.

## Inputs

- Connected calendars and their live events.
- Configured working hours, buffers, travel time, and appointment types per person.
- Branch, team, and seat structure from WARDEN's `org-structure-manager`.
- Outstanding holds placed by ECHO's `two-slot-closer` and by [`multi-party-coordinator`](../multi-party-coordinator/SKILL.md).

## Procedure

1. **Compute availability from live calendar state**, not from a periodically refreshed cache — a slot that was free ten minutes ago is not an answer.
2. **Apply working hours, buffers, and travel time as part of availability**, so a returned slot is genuinely bookable rather than merely empty.
3. **Resolve every slot in both the viewer's and the contact's timezone**, and never return a bare local time.
4. **Honor holds as unavailable** for as long as they stand, and expire them on their own window rather than leaving them indefinitely.
5. **Return no availability rather than a stale answer** where a calendar connection is degraded, and say the connection is degraded.
6. **Drop a seat from availability the moment WARDEN reports it revoked**, and surface the appointments already booked against it.
7. **Serve every other agent from this one computation** rather than allowing a local availability table anywhere in the platform.

## Output

- Live availability per person, with buffers and travel applied, resolved in both timezones.
- Hold state, with each hold's owner and expiry.
- A degraded-connection state, distinct from an empty calendar.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TEMPO — these apply to every TEMPO skill, per `AGENTS.md` §5:**

- No-show recovery attempts happen **within minutes**, never deferred to the next day.
- Every auto-generated task carries a **real owner, a real due date, and context** — never a bare reminder with no accountable party.
- Tasks blocking money are **escalated**, not left in the standard queue to age out.

**Specific to this skill:**

- **A degraded calendar connection returns "unknown," never "free."** An unreachable calendar rendered as open availability books a producer into a slot they are already in, and it does so most often during exactly the outage that made the calendar unreachable.
- **No other agent keeps its own availability table.** SCOUT's router reads shift state here, ECHO's closer reads bookable slots here, and a second copy anywhere disagrees with this one the first time someone changes their hours.
- **A hold makes a slot unavailable and expires on its own window.** Holds that never expire fill a calendar with appointments nobody booked; holds that are not honored offer the same slot to two people.
- **Buffers and travel time are part of availability, not a preference layered on top.** A back-to-back booking across town is a missed appointment that the calendar said was fine.
- **Every slot carries both timezones.** A time offered in the office's timezone is an error the contact has to catch.
- **A revoked seat leaves availability the same day, and its existing bookings are surfaced rather than silently dropped.** WARDEN revokes access on departure; the appointments that person had booked with customers do not disappear because their login did.
- **Availability is never adjusted to make a lead routable.** If nobody is genuinely free, that is the answer, and SCOUT's router overflows on it rather than being told a fiction.

## Measured on

Availability accuracy against actual bookability · double-bookings (target zero) · slots served from a degraded connection as free (target zero) · holds expired rather than stranded
