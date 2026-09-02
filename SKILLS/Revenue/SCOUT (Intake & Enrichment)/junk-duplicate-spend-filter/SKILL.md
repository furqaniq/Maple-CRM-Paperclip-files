---
name: junk-duplicate-spend-filter
description: Flags bot fills, junk, and duplicate paid spend before they pollute the database or the attribution model. Runs on every arriving record, ahead of assignment.
agent: SCOUT
division: Revenue
binding: mandate
---

# Junk & Duplicate-Spend Filter

A junk record costs a little; a junk record inside the attribution model costs every decision made from it.

## When this fires

- On every record arriving, before assignment.
- On a duplicate arrival reported by [`identity-resolver`](../identity-resolver/SKILL.md).
- On the review cycle over records previously rejected, to catch the filter's own false positives.

## Inputs

- The normalized record and its submission signals — timing, sequence, device and form fingerprint, source.
- Known junk and bot patterns, and the account's own confirmed-junk history.
- Paid-source spend and campaign identifiers, for duplicate-spend detection.
- Human dispositions on previously rejected records.

## Procedure

1. **Score the record on submission-behavior signals** — impossible timing, sequence artifacts, known bot fingerprints, non-deliverable contact details, contradiction between fields.
2. **Reject only above the confidence threshold**, and hold everything below it as suspect rather than rejecting it.
3. **Keep a rejected record retrievable with its rejection reason.** A rejection is a routing state, not a deletion.
4. **Report a duplicate paid arrival to LEDGER's `event-stream-attribution`**, which holds spend by channel and campaign, so the same person bought twice is visible as spend rather than as two leads.
5. **Sample rejections back to a human on the review cycle**, because a filter nobody audits drifts and takes real leads with it.
6. **Feed confirmed dispositions back into the patterns**, using confirmed outcomes rather than the filter's own prior guesses.

## Output

- A disposition per record: clean, suspect, or rejected, with the specific signal that produced it.
- A duplicate-paid-spend notice to attribution and billing.
- A sampled rejection review with the false-positive rate stated.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SCOUT — these apply to every SCOUT skill, per `AGENTS.md` §5:**

- SCOUT **never pulls, requests, infers, or stores consumer credit information**, under any circumstance.
- Position estimates are **always labeled as estimates** — never presented as fact.

**Specific to this skill:**

- **Rejection is reversible and reasoned.** A record deleted on a filter's judgment is a real customer the company can never recover and will never know it lost.
- **A suspect record is held for review, not silently dropped and not quietly worked.** Both failure modes look like success from the outside.
- **The filter reads submission behavior, never who the person appears to be.** Name, apparent ethnicity, neighborhood, language, and email domain are not junk signals, and a filter that learns them becomes a screen on protected characteristics that nobody wrote down and nobody can see.
- **The false-positive rate is measured and reported, not assumed.** A filter reporting only its catch rate is reporting half of its effect, and the expensive half is the other one.
- **Duplicate paid spend is reported as spend, not resolved as a lead-count adjustment.** The point of catching it is that someone stops paying for it.
- **Feedback comes from confirmed human dispositions only.** A filter trained on its own rejections converges on whatever it already believed, and the drift is invisible because the evidence is its own output.

## Measured on

Junk catch rate · false-positive rate on sampled rejections · duplicate paid spend surfaced · junk records reaching the attribution model (target zero)
