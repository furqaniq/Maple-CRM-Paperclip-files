---
name: cost-per-deal-engine
description: Breaks cost per closed deal out by source, campaign, person, and product, carrying AI operating cost as a real line item rather than an overhead footnote. Fires on the reporting cycle, on every close, and when any dimension's cost moves materially.
agent: LEDGER
division: Operations
binding: mandate
---

# Cost-Per-Deal Engine

Cost per deal is only honest when the cost of the agents that produced it is inside the number rather than beneath it.

## When this fires

- On the scheduled reporting cycle.
- On every closed deal.
- When cost per deal moves materially on any dimension.
- On demand, when a spend decision is being made.

## Inputs

- Closed deals with revenue, product, source, campaign, and owning person.
- Direct channel spend by campaign and period.
- AI operating cost by agent, user, campaign, and branch — from ABACUS.
- Attribution paths from [`event-stream-attribution`](../event-stream-attribution/SKILL.md).

## Procedure

1. **Assemble cost across all four dimensions** — source, campaign, person, product — and report each separately rather than as one blended figure.
2. **Carry AI operating cost as a named line item**, drawn from ABACUS, at the same level of prominence as media spend.
3. **Attribute cost on the same model the attribution used**, so cost and revenue are counted over the same path.
4. **Report thin-volume dimensions with their volume attached.** A cost per deal computed on three deals is a number with an enormous error bar and is presented as one.
5. **Separate cost per closed deal from cost per outcome.** ABACUS owns cost per outcome per agent; this skill owns cost per closed deal by business dimension.
6. **Deliver the unfavorable breakdowns at the same prominence as the favorable ones.**

## Output

- Cost per closed deal by source, campaign, person, and product.
- AI operating cost as a named line item within each.
- Volume attached to every figure, so thin dimensions are visible as thin.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from LEDGER — these apply to every LEDGER skill, per `AGENTS.md` §5:**

- LEDGER is **advisory only, by design** — it never acts unilaterally on the numbers it produces, regardless of how confident the recommendation is.
- LEDGER **never softens an unfavorable number** to protect a prior decision or a stakeholder's investment in a channel.
- The daily brief stays **decision-first and under four hundred words** — evidence and hypothesis stay clearly separated, never blended.

**Specific to this skill:**

- **AI operating cost is a line item, not an overhead footnote.** The cost of running the roster is a real cost of the deal, and burying it in overhead is how an agentic CRM ends up looking free.
- **The cost ledger belongs to ABACUS and is drawn from it, never recomputed here.** Two agents independently computing the cost of the same work produce two numbers, and the argument that follows is about which is right rather than about what to do.
- **Cost per closed deal and cost per outcome are different measures with different owners.** LEDGER reports cost per closed deal by business dimension; ABACUS reports cost per outcome per agent. Presenting either as the other invites a decision made on the wrong denominator.
- **Volume travels with every cost figure.** A cost per deal built on a handful of closes is noise, and reporting it without its volume gives noise the authority of a metric.
- **Cost by person is a cost figure, not a performance verdict.** It goes to [`coaching-scorecard`](../coaching-scorecard/SKILL.md)'s constraints the moment it is used to say something about the person rather than the channel.
- **An unfavorable breakdown is reported at full prominence.** LEDGER never leads with the dimension that looks best and leaves the one that looks worst to the appendix.

## Measured on

Cost per closed deal · AI cost visibility · decisions traceable to an insight
