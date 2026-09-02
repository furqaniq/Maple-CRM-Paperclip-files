---
name: pipeline-sourced-reporting
description: Reports in leads and pipeline sourced from social, with attribution stated on its basis and untraceable leads left unclaimed. Fires on the reporting cycle, at campaign close, and on demand.
agent: BEACON
division: Marketing
binding: mandate
---

# Pipeline-Sourced Reporting

An untraceable lead is not claimed. “Likely from social” is reported as unattributed.

## When this fires

- On the scheduled reporting cycle.
- At campaign close.
- On demand from ATLAS or the Account Owner.
- Immediately, when a compliance event materially changes what the period contained.

## Inputs

- Leads routed from social, with their originating threads and items.
- The CRM outcome for each routed lead.
- Publishing consistency against the calendar: slots filled, missed, and dead weeks.
- Inbox response times and escalation times against the hour.
- Escalations raised and how each closed.
- Cost draw from ABACUS's `real-time-consumption-meter`, the account's single cost ledger.

## Procedure

1. **Report leads sourced and pipeline created**, each traced to the specific item that produced it.
2. **Report publishing consistency against the calendar** — slots filled, slots missed, and gaps that became dead weeks.
3. **Report inbox response time against the KPI**, and escalation times against the hour, including the ones that ran over.
4. **Report what did not work**: withdrawn posts, blocked assets, complaints, unanswered threads, failed publishes.
5. **State attribution on its basis.** A lead that cannot be traced to a specific social item is reported as unattributed, not assigned to social on plausibility.
6. **Attach cost** from ABACUS's meter — the account's single cost ledger — never recomputed here.

## Output

A period report leading with leads sourced and pipeline created against traceable items, publishing consistency including missed slots, response and escalation times including overruns, compliance events at full size, unattributed leads left unattributed, and cost drawn from the ledger.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from BEACON — these apply to every BEACON skill, per `AGENTS.md` §5:**

- Genuine leads surfacing in the social inbox are **routed into the CRM**, never left to die inside the platform.
- Anything in mentions, reviews, or competitor activity that needs a human is escalated **within the hour**.
- Reporting is always in **leads and pipeline**, never vanity metrics.

**Specific to this skill:**

- **Impressions, reach, likes, and follower counts are not results.** They are never the headline and never stand in for pipeline; where they appear at all, it is as context beneath something that reached the CRM.
- **An untraceable lead is not claimed.** Attribution is stated with its basis, and "likely from social" is reported as unattributed. Claiming plausible attribution is how a social report becomes the one number in the company nobody believes.
- **Missed slots, dead weeks, blocked assets, and unanswered inbox items appear in the report.** A consistency KPI computed only over the weeks that went well is not a KPI.
- **Compliance events are reported as themselves** — a complaint, a withdrawn post, an AEGIS block — never folded into a performance figure or netted against a good week.
- **BEACON reports; it does not argue.** The report is a record of what happened, not a case for BEACON's budget, cadence, or autonomy level.

## Measured on

Leads sourced from social · publishing consistency · inbox response time
