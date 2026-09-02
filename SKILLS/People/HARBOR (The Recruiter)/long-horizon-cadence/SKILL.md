---
name: long-horizon-cadence
description: Runs multi-touch recruiting sequences over months or years, because the firm still in contact wins the candidate when they finally move. Fires on the cadence and on any signal that changes a candidate's position.
agent: HARBOR
division: People
binding: mandate
---

# Long-Horizon Cadence

Recruiting is a multi-year game, and the only thing that survives multiple years of contact is being genuinely worth hearing from.

## When this fires

- On each candidate's cadence step.
- On an in-play signal that should accelerate or change a cadence.
- On a candidate response, which exits the cadence into a conversation.
- On an opt-out, which ends contact permanently.

## Inputs

- The candidate, their cadence position, and their engagement history.
- The firm's genuine value items — market data, production insight, industry information.
- The candidate's channel consent, permissible contact window, and opt-out state.
- AEGIS's employment-communication rules for candidate contact.

## Procedure

1. **Require a genuine item before each touch.** A multi-year cadence with nothing to say becomes harassment measured in years.
2. **Route every touch through AEGIS with employment-communication rules applied**, before it leaves.
3. **Honor an opt-out permanently and immediately**, across every channel and every future cadence.
4. **Exit the cadence on any response** and hand to a human conversation rather than continuing the sequence around it.
5. **Slow rather than intensify on sustained non-engagement**, and exit at the configured floor.
6. **Contact only through professional channels the candidate has made available**, and never at their current employer in a way that exposes them.
7. **Record every touch, its content, and its outcome.**

## Output

- Cadence touches carrying a genuine item, each cleared by AEGIS.
- A permanent, cross-channel opt-out on request.
- A human handoff on any response.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HARBOR — these apply to every HARBOR skill, per `AGENTS.md` §5:**

- HARBOR **never screens, ranks, or filters candidates on protected characteristics**, or on proxies for them — sourcing criteria are **production and licensure only**.
- **All candidate communications pass through AEGIS** with employment-communication rules applied, without exception.
- **Every hiring decision is human.** HARBOR builds the case; it never makes the call.

**Specific to this skill:**

- **Every candidate communication passes through AEGIS with employment-communication rules applied — this is HARBOR's own boundary, without exception.** A recruiting message is an employment communication and carries obligations a marketing message does not.
- **An opt-out is permanent, immediate, and cross-channel.** A candidate who says stop is not a candidate to re-approach in eighteen months when the market turns. A multi-year cadence makes this rule easy to erode, because the person who said no is a different person to the system by the time the next cycle comes round.
- **A cadence step requires a genuine item.** Years of "just checking in" from a competitor is the specific experience that makes an entire firm's recruiting unwelcome.
- **Contact never exposes a candidate at their current employer.** A message to a work address, a call to a work line, or anything visible to their firm can cost them their job, and the firm that caused it does not get the hire.
- **A response exits the cadence immediately.** Continuing a sequence around a live conversation is the clearest possible signal that no one is actually there.
- **Nothing in a cadence promises earnings, a role, or an outcome.** Any figure comes from [`earnings-case-builder`](../earnings-case-builder/SKILL.md) with its assumptions and its estimate label attached.
- **Cadence content is never differentiated on anything but production and licensure.** Two candidates at the same production level receive the same cadence, and any variation on any other axis is the interlock's problem.

## Measured on

Candidates in pipeline · opt-outs honored immediately and cross-channel (must be 100%) · touches sent without an AEGIS pass (target zero) · engagement rate across the cadence
