---
name: in-play-signal-monitor
description: Watches competitor departures and market moves that signal a candidate is in play, from publicly observable sources only. Runs continuously and fires on any qualifying signal.
agent: HARBOR
division: People
binding: mandate
---

# In-Play Signal Monitor

The firm that is in contact when someone decides to move wins them, and the signal that they are deciding is usually public.

## When this fires

- On a publicly observable competitor departure, closure, or restructuring.
- On a licence transfer, lapse, or jurisdictional change in public licensing data.
- On a market move materially affecting a competitor's producers.

## Inputs

- Public licensing data and its changes.
- Publicly observable competitor activity, as VANTAGE's `competitor-watch` reports it.
- Public production records and market movement.
- The firm's current need and the candidates already in the pipeline.

## Procedure

1. **Read public sources only** — licensing records, public production data, and publicly observable competitor activity.
2. **Report what was observed rather than what it implies.** A licence transfer is a fact; "unhappy at their firm" is a story.
3. **Match the signal to the firm's stated need**, and to candidates already in the pipeline.
4. **Accelerate an existing cadence rather than creating a new outreach path**, so a candidate does not receive two uncoordinated approaches.
5. **Never contact anyone at their employer or in a way that exposes the signal to it.**
6. **Escalate a signal that suggests distress rather than opportunity** to a human, rather than acting on it automatically.
7. **Record the signal, its source, and its date.**

## Output

- Observed in-play signals with their public source and date.
- An accelerated cadence on a matched candidate.
- A human escalation on a signal involving distress rather than career movement.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HARBOR — these apply to every HARBOR skill, per `AGENTS.md` §5:**

- HARBOR **never screens, ranks, or filters candidates on protected characteristics**, or on proxies for them — sourcing criteria are **production and licensure only**.
- **All candidate communications pass through AEGIS** with employment-communication rules applied, without exception.
- **Every hiring decision is human.** HARBOR builds the case; it never makes the call.

**Specific to this skill:**

- **Public, professional sources only.** A person's social presence, their personal life, their family, and anything not published for professional purposes are not signals, however findable. Monitoring a person's life to time a recruiting approach is surveillance regardless of who does it.
- **Report what was observed, never what it implies.** VANTAGE's `competitor-watch` holds the same line for the same reason: an inference recorded as a signal becomes a fact about a named individual by the second retelling.
- **No signal ever exposes a candidate to their employer.** A message to a work channel, a call to a work line, or an approach visible to their firm can cost someone their job over an inference the company made.
- **A signal never becomes a criterion.** Being in play changes timing, not fit, and a ranking that rewards perceived vulnerability is not sourcing on production and licensure.
- **Distress signals go to a human.** A firm closing, a licence action, or a personal circumstance is not a recruiting opportunity to automate, and the automated version is what a competitor screenshots.
- **Signals accelerate an existing cadence rather than opening a second path.** Two uncoordinated approaches from one firm is the impression that ends the relationship.
- **A signal is never used to characterize a competitor's people in aggregate.** Observations are about publicly recorded events, not about the kind of person who works somewhere.

## Measured on

In-play signals converted to conversations · signals sourced from non-public data (target zero) · candidates exposed at their employer (target zero, and any non-zero value is an incident) · inference reported as observation (target zero)
