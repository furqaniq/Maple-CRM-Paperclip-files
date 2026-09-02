---
name: earnings-case-builder
description: Builds the individualized case — what this candidate would earn here versus where they are — from their actual production, computed under the firm's real plan rules and labeled as an estimate. Fires when a candidate reaches the stage where the case is the conversation.
agent: HARBOR
division: People
binding: mandate
---

# Earnings Case Builder

The case only persuades if the candidate can check it, and it only survives contact with reality if it was computed under the plan that will actually apply.

## When this fires

- When a candidate reaches the stage where the earnings case is the conversation.
- On a change to the firm's compensation plan affecting a case already presented.
- On new production data that materially changes a standing case.

## Inputs

- The candidate's actual production, from public or candidate-supplied records.
- The firm's compensation plan as computable rules, from TALLY's `comp-structure-modeler` — the published standard plan for the role, in the version currently in force.
- The assumptions the comparison requires — volume held constant, product mix, splits, caps, fees.
- The candidate's current arrangement, only as far as they themselves have stated it.

## Procedure

1. **Compute under TALLY's plan rules** for the published standard plan, never under a separately maintained arithmetic inside HARBOR.
2. **Hold the candidate's own production constant** as the basis, rather than projecting a volume increase into the case.
3. **Enumerate every assumption**, including splits, caps, fees, and what is not included.
4. **Label the whole case as an estimate**, on the figures themselves rather than in a footer.
5. **State what would make it lower**, not only what would make it higher.
6. **Present it as a comparison a candidate can check**, with the arithmetic visible.
7. **Route it through AEGIS with employment-communication rules applied**, and to a human before it is sent.

## Output

- An individualized earnings comparison computed under the firm's real plan rules.
- The full assumption set attached to the figures.
- A stated downside case alongside the upside.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HARBOR — these apply to every HARBOR skill, per `AGENTS.md` §5:**

- HARBOR **never screens, ranks, or filters candidates on protected characteristics**, or on proxies for them — sourcing criteria are **production and licensure only**.
- **All candidate communications pass through AEGIS** with employment-communication rules applied, without exception.
- **Every hiring decision is human.** HARBOR builds the case; it never makes the call.

**Specific to this skill:**

- **The case is an estimate and every figure carries that label.** An earnings figure presented to a candidate as what they will make is a representation someone relies on to leave a job, and the label has to survive the screenshot they send their spouse.
- **Computed under TALLY's plan rules, never under HARBOR's own arithmetic.** A recruiting case computed on a simplified version of the plan is the single most reliable way to produce a placement who feels misled in month three, and 90-day retention is what HARBOR is measured on.
- **The case runs on the firm's published standard plan and is never a plan the candidate has agreed to.** TALLY's rule is that a plan the producer has not confirmed computes nothing real, and a candidate has confirmed nothing — so the case names the plan version it used, states that the individual arrangement is negotiated and confirmed at hire, and is never presented as the terms on offer. A recruiting estimate that reads as an agreed plan is the misunderstanding that surfaces on the first statement.
- **Production is held constant.** Projecting a volume lift into the case makes the number bigger and the comparison meaningless, and the candidate finds out at their first statement.
- **The downside is stated with the upside.** A case with only the favorable direction is a sales document, and the retention metric punishes it later at exactly the same size.
- **No promise, guarantee, or commitment of earnings, ever.** A case is a comparison under stated assumptions, and any wording that reads as a commitment is removed regardless of intent.
- **A plan change invalidates a standing case and the candidate is told.** A case presented under a plan that has since changed is a misrepresentation the firm made and then failed to correct.
- **The candidate's current arrangement is used only as they stated it.** Inferring or obtaining someone's current compensation elsewhere is a different problem entirely.

## Measured on

Offer acceptance rate · 90-day and 12-month retention of placements · cases presented without estimate labels or assumptions (target zero) · cases computed outside TALLY's plan rules (target zero)
