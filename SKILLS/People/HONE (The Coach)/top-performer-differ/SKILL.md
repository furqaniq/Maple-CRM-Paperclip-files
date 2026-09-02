---
name: top-performer-differ
description: Identifies the specific behavior separating top performers from the rest using the firm's own closed-won data rather than received wisdom. Fires on the analysis cycle and when the performance distribution shifts.
agent: HONE
division: People
binding: mandate
---

# Top-Performer Differ

Every firm has a story about what its best people do differently, and the closed-won data usually disagrees with it.

## When this fires

- On the analysis cycle.
- When the performance distribution shifts materially.
- When a new behavior enters the playbook and its effect is testable.

## Inputs

- The firm's own closed-won and closed-lost data.
- Scored conversation behavior from [`playbook-scorer`](../playbook-scorer/SKILL.md), with coverage.
- Conditions per person — territory, lead volume, lead quality, vintage mix — from LEDGER's `vintage-cohort-analyzer`.
- The firm's existing beliefs about what separates its top performers, stated explicitly so they can be tested.

## Procedure

1. **Normalize for conditions first.** Without this the analysis identifies the best territory and calls it the best behavior.
2. **Identify behaviors that differ**, at the level of a specific action rather than a trait.
3. **Test each candidate behavior against outcomes**, and report effect size rather than presence or absence.
4. **Separate evidence from hypothesis explicitly**, and never blend them.
5. **Report where the firm's stated belief is not supported**, including when it is the manager's own.
6. **Check that the identified behavior is coachable** — something a person can do differently on Monday, not a characteristic.
7. **Hand supported behaviors to [`coaching-plan-builder`](../coaching-plan-builder/SKILL.md)**, and unsupported ones to the manager as a finding.

## Output

- Named behaviors with their measured effect on outcomes, evidence separate from hypothesis.
- An explicit result on each of the firm's stated beliefs, including the unsupported ones.
- A coachability check on every identified behavior.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from HONE — these apply to every HONE skill, per `AGENTS.md` §5:**

- HONE is **advisory and reports to the manager, never directly to the producer.**
- It **never produces automated performance rankings** that could drive an employment decision without human review, and **never scores anyone on protected characteristics or proxies**.
- HONE **never makes or recommends a termination decision** — that stays entirely with the manager.

**Specific to this skill:**

- **Behaviors, never traits.** "Confidence," "hunger," "presence," and "culture fit" are not behaviors — they are impressions of a person, they are not coachable, and they carry every bias the data was supposed to remove. What survives is what someone actually did: when they followed up, what they asked, how they opened.
- **Conditions are normalized before anything is compared.** LEDGER's `coaching-scorecard` holds the same line, and unnormalized this analysis reliably concludes that the best territory has the best behavior.
- **Evidence and hypothesis stay separate.** A hypothesis about why someone succeeds, stated with the authority of the number beside it, is a fact about a person by the second retelling.
- **An unsupported belief is reported as unsupported, including the manager's own.** The whole point of using closed-won data is to test received wisdom, and softening the result on the wisdom the audience holds returns the firm to instinct with a report attached.
- **No behavior is identified from a protected characteristic or a proxy.** A finding that top performers share a demographic is a finding about the firm's lead routing, not about behavior, and it is escalated rather than reported as coaching.
- **Effect size is reported, not just direction.** A behavior with a real but tiny effect is not the one thing to change this week.
- **This is analysis for coaching, never a ranking.** A list of who does the top behaviors is a leaderboard, and it is exactly what the interlock forbids.

## Measured on

Improvement on the coached behavior · variance between top and median performer · behaviors identified as traits rather than actions (target zero) · stated beliefs tested and reported honestly
