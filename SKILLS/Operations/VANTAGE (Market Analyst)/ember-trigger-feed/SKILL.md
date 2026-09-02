---
name: ember-trigger-feed
description: Pushes live market conditions into EMBER's trigger logic so a reactivation fires on a real change rather than an arbitrary calendar date, and stops triggering entirely when the feed goes stale. Fires on every qualifying market change and on every staleness event.
agent: VANTAGE
division: Operations
binding: mandate
---

# EMBER Trigger Feed

A reactivation is an outbound message to a real person. It fires on a condition that is actually true right now, or it does not fire.

## When this fires

- On every market change that crosses a configured trigger condition.
- On the feed going stale for any measure a live trigger depends on.
- When a trigger's underlying geography loses coverage.
- When a condition that fired a trigger reverses before the send goes out.

## Inputs

- The live market feed with its per-measure freshness state.
- EMBER's configured trigger conditions and the geographies they cover.
- The contact-level geography each trigger would evaluate against.
- Recent trigger history, to detect a condition oscillating across a threshold.

## Procedure

1. **Evaluate trigger conditions only against figures inside their freshness window.**
2. **Withhold the trigger where the feed is stale**, and tell EMBER the feed is stale rather than simply sending nothing.
3. **Match the trigger to the contact's actual geography**, at the granularity the condition was defined at.
4. **Suppress an oscillating condition.** A measure crossing a threshold back and forth fires repeated reactivations at the same person.
5. **Withdraw a trigger whose condition has reversed** before the send leaves, and tell EMBER it was withdrawn.
6. **Pass the condition, the figures, the source, and the as-of date with the trigger**, so the message can be grounded in something citable.
7. **Never characterize what the condition means for the contact.** VANTAGE supplies the fact; EMBER and AEGIS own what is said about it.

## Output

- A trigger event carrying the condition, the figures, the source, and the as-of date.
- An explicit stale-feed notice to EMBER where a trigger was withheld.
- A withdrawal where a condition reversed before the send.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VANTAGE — these apply to every VANTAGE skill, per `AGENTS.md` §5:**

- VANTAGE **reports conditions and cites sources** — it **never forecasts future rates or values as fact**.
- Every projection is **labeled as an estimate with its assumptions visible**, without exception.
- Anything in the market that changes the plan gets **briefed to the owner**, not buried in a routine report.

**Specific to this skill:**

- **A stale feed stops triggering. It never triggers on the last known value.** A reactivation is an outbound message about a market condition to a real person — firing it on a rate that moved yesterday tells that person something untrue about their own money, and it goes out under the company's name.
- **Withholding is announced, not silent.** EMBER is told the feed went stale rather than left to interpret an absence of triggers as an absence of market movement, which is the same information with the opposite meaning.
- **A reversed condition withdraws the trigger before the send.** A message about a rate change that has already unwound arrives as a company that does not know what the market is doing.
- **An oscillating condition is suppressed, not fired repeatedly.** A measure crossing a threshold four times in a week produces four reactivations to the same contact, and no suppression rule downstream is looking for that shape.
- **The trigger carries its figures, source, and as-of date.** A reactivation that cites a market condition needs the condition citable, or the claim in the message has nothing behind it.
- **VANTAGE supplies conditions; it never says what they mean for a contact.** "Rates in this ZIP moved this much, as of this date, per this source" is the whole contribution. Whether that is good news for a specific borrower is an eligibility-adjacent statement, and it belongs to the agents the gate covers.

## Measured on

Trigger precision — reactivations fired that converted · triggers fired on stale data (target zero) · data freshness at trigger time
