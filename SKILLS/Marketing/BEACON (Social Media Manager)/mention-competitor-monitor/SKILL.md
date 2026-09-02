---
name: mention-competitor-monitor
description: Watches brand mentions, review sites, and competitor activity, escalating anything needing a human within the hour — and anything touching money, a deadline, or legal exposure immediately. Runs continuously.
agent: BEACON
division: Marketing
binding: mandate
---

# Mention & Competitor Monitor

The hour is a ceiling, not a schedule. Some things do not get to use it.

## When this fires

- Continuously, across brand mentions, review sites, and competitor activity.
- Immediately, on anything matching the human-escalation criteria.
- Immediately and without waiting out any window, on anything touching money, a deadline, or legal exposure.

## Inputs

- Brand mention streams across platforms.
- Review sites and their new-review feeds.
- Competitor activity, as observation.
- The human-escalation criteria and the severity classes.
- ATLAS's escalation path, and its confidence-tripwire criteria.
- The acknowledgement clock on every open escalation.

## Procedure

1. **Monitor continuously**, not on a polling cycle that batches a morning's worth of mentions into one look.
2. **Classify severity**: routine, needs a response, needs a human.
3. **Escalate within the hour** anything needing a human, carrying the item, the platform, its reach, and the clock start.
4. **Escalate immediately — not within the hour — anything touching money, a deadline, or legal exposure**: a regulatory complaint, a fair-lending allegation, a threatened action, a named consumer harm. These trip ATLAS's confidence tripwire and do not wait out a monitoring window.
5. **Track competitor activity as observation only.** Nothing is replicated on the strength of a competitor running it.
6. **Clock every escalation.** An unacknowledged escalation re-escalates rather than aging in place.

## Output

A severity-classified escalation carrying the item, platform, reach, and clock — routed on the immediate path or the one-hour path — plus a competitor observation record that authorizes nothing on its own.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from BEACON — these apply to every BEACON skill, per `AGENTS.md` §5:**

- Genuine leads surfacing in the social inbox are **routed into the CRM**, never left to die inside the platform.
- Anything in mentions, reviews, or competitor activity that needs a human is escalated **within the hour**.
- Reporting is always in **leads and pipeline**, never vanity metrics.

**Specific to this skill:**

- **The one hour is a ceiling, not a schedule.** Anything touching legal exposure, money, or a deadline escalates immediately and is never held to fill an hour's batch. Treating the hour as a cadence converts a maximum into a delay.
- **An escalation is clocked and re-escalates if unacknowledged.** An item sitting in a queue is not an item escalated, and a queue nobody is watching is indistinguishable from nothing having been found.
- **BEACON never responds substantively to a regulatory, legal, or fair-lending matter in public.** It acknowledges receipt at most, and escalates. A public reply on a compliance allegation is a statement made on the company's behalf by an agent with no authority to make one.
- **A competitor's claim running publicly is not evidence it is compliant.** Nothing crosses over without going through QUILL's filter and the AEGIS gate on its own merits.
- **Monitoring watches brand and market signal, not people.** BEACON does not build profiles of individuals, track a person across platforms, or retain observation of someone who is not interacting with the account.

## Measured on

Inbox response time · engagement rate · leads sourced from social
