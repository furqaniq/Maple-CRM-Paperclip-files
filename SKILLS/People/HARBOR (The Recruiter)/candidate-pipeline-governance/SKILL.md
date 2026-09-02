---
name: candidate-pipeline-governance
description: Tracks every candidate through a dedicated pipeline with stage governance, so nobody goes quiet unnoticed. Runs continuously against every candidate in the pipeline.
agent: HARBOR
division: People
binding: mandate
---

# Candidate Pipeline Governance

Recruiting fails the same way lead management used to — quietly, one candidate at a time, in the gap after the good conversation.

## When this fires

- On every candidate entering the pipeline.
- On every stage transition, and on any candidate exceeding expected dwell in a stage.
- On a candidate going quiet past their stage's threshold.

## Inputs

- Every candidate, their stage, and their time in it.
- Stage entry and exit criteria for the recruiting pipeline.
- Last contact and last response per candidate.
- The owning recruiter or manager, from WARDEN's `org-structure-manager`.

## Procedure

1. **Hold every candidate in a named stage with a named owner**, never in an undifferentiated pool.
2. **Enforce stage criteria and name the unmet requirement** on a blocked transition, as FORGE's gate does on a file.
3. **Flag a candidate going quiet past their stage's threshold**, with what the next action is.
4. **Keep candidate data separate from the customer CRM**, so a candidate is never contacted as a lead and a lead is never worked as a candidate.
5. **Reassign rather than orphan** where an owner leaves or changes role.
6. **Close a candidate with a recorded reason** — placed, withdrew, declined, or not progressed — rather than letting them lapse.
7. **Surface the pipeline's own gaps**, where a stage holds candidates nobody has touched.

## Output

- A stage-governed pipeline with a named owner per candidate.
- Quiet-candidate flags with the specific next action.
- Closed candidates with a recorded reason.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HARBOR — these apply to every HARBOR skill, per `AGENTS.md` §5:**

- HARBOR **never screens, ranks, or filters candidates on protected characteristics**, or on proxies for them — sourcing criteria are **production and licensure only**.
- **All candidate communications pass through AEGIS** with employment-communication rules applied, without exception.
- **Every hiring decision is human.** HARBOR builds the case; it never makes the call.

**Specific to this skill:**

- **Candidate data lives separately from the customer CRM and never crosses.** A candidate appearing in a marketing segment is a person's job search disclosed by a mailing list, and a customer worked as a recruiting target is worse.
- **Every candidate has a named live owner.** A candidate owned by a departed recruiter is exactly the candidate who goes quiet unnoticed, which is the failure this skill exists to prevent.
- **A closed candidate carries a recorded reason.** A lapsed candidate with no reason cannot be re-approached intelligently, cannot be reviewed for bias in aggregate, and looks identical to one who was never worked.
- **A stage block names the unmet requirement.** A candidate stuck with no stated reason is a person waiting on a firm that has, from their side, simply stopped replying.
- **Stage governance never encodes anything but production, licensure, and process state.** A stage criterion that filters on anything else is the interlock's problem arriving through the pipeline configuration.
- **Quiet flags route to the owner, never to the candidate.** "We noticed you have gone quiet" is a message the firm should think about before sending automatically.
- **The pipeline is never used to rank people for anything but this process.** A recruiting pipeline is not a bench, a watchlist, or a source of comparisons about current employees.

## Measured on

Candidates in pipeline · candidates going quiet unnoticed (target zero) · candidates owned by a departed seat (target zero) · closures carrying a recorded reason
