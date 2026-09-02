---
name: focus-block-defender
description: Protects deep-work blocks from being consumed by low-value meetings, without ever standing between someone and an urgent escalation. Fires on every booking attempt that would land inside a protected block.
agent: TEMPO
division: Revenue
binding: mandate
---

# Focus-Block Defender

A protected block that also blocks the urgent thing is not protection, it is a delay with a wellness justification.

## When this fires

- On any booking attempt landing inside a protected block.
- On the block-planning cycle, where blocks are proposed from observed work patterns.
- When protected time has been consumed repeatedly, as a pattern to surface.

## Inputs

- The protected block definitions, and who set each one.
- The booking attempt — its type, its value, and who requested it.
- The account's configured urgency classes that override protection.
- Observed consumption of protected time over the period.

## Procedure

1. **Classify the booking attempt** against the account's urgency classes before applying any protection.
2. **Let urgent classes through immediately** — a distress escalation, a money-blocking escalation, a same-day closing item, a customer already on the line.
3. **Offer the nearest slot outside the block** for everything else, rather than simply declining.
4. **Let the person override their own block instantly and without friction**, and never require a justification.
5. **Never protect a block against the person who owns it.**
6. **Surface repeated consumption as a pattern** to the owner and their manager, as information rather than an enforcement action.
7. **Propose blocks from observed work patterns**, not from a generic template applied to everyone.

## Output

- A redirected booking to the nearest slot outside the block.
- An immediate pass-through for anything in an urgency class.
- A consumption pattern surfaced to the owner.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TEMPO — these apply to every TEMPO skill, per `AGENTS.md` §5:**

- No-show recovery attempts happen **within minutes**, never deferred to the next day.
- Every auto-generated task carries a **real owner, a real due date, and context** — never a bare reminder with no accountable party.
- Tasks blocking money are **escalated**, not left in the standard queue to age out.

**Specific to this skill:**

- **Protection never stands between a person and an urgent escalation.** A distress escalation on ECHO's same-minute clock, a money-blocking escalation, a customer on a held line — each passes straight through. A focus block that delays these is the mechanism by which a well-meant feature causes the harm the escalation existed to prevent.
- **The owner overrides their own block instantly, with no friction and no justification.** A protection someone has to argue with is a protection they will turn off entirely.
- **A declined booking is redirected, never simply refused.** Refusing without offering an alternative pushes the meeting outside the platform, where nothing can see it.
- **Blocks are proposed from observed patterns, never imposed from a template.** A generic focus block dropped on everyone's calendar is ignored within a week and then ignored permanently.
- **Consumption patterns are surfaced as information, not enforced.** How someone spends their own time is not a compliance matter, and a defender that starts refusing on their behalf has changed jobs.
- **Protection is never used to hide availability from a colleague.** A block is time set aside for work, not a routing mechanism, and using it as one puts a second availability model in the platform.
- **A block never blocks a customer-facing commitment already made.** The commitment came first and the customer was told.

## Measured on

Protected time preserved · urgent escalations delayed by a block (target zero) · redirected bookings accepted outside the block · blocks overridden by their own owner
