---
name: geo-granular-market-feed
description: Tracks inventory, days on market, absorption, and price and rate movement at metro, county, ZIP, and neighborhood level, with every figure carrying its source and its as-of date. Runs continuously against every source and fires on any staleness or divergence.
agent: VANTAGE
division: Operations
binding: mandate
---

# Geo-Granular Market Feed

Every other agent reasons from the company's own data. This is the feed that brings the outside world in, and it is only useful if its freshness is never in doubt.

## When this fires

- Continuously, on every source refresh.
- When a source goes stale past its defined window.
- When two sources materially disagree on the same measure.
- When a geography the business operates in has no coverage at all.

## Inputs

- External market data sources, with their publication cadence and lag.
- The geographies the business actually operates in, at the granularity it operates at.
- Per-measure freshness windows — a rate goes stale in hours, an absorption rate in weeks.
- Source reliability history and known divergences.

## Procedure

1. **Attach the source and the as-of date to every figure**, at the point it enters the feed, not at the point it is reported.
2. **Apply a freshness window per measure**, since measures go stale at completely different rates.
3. **Mark a figure past its window as stale and stop serving it as current.**
4. **Report source disagreement as disagreement**, with both figures and both sources, rather than averaging or picking.
5. **Report a geography with no coverage as uncovered.** Substituting the enclosing county for a neighborhood is a fabrication with a plausible number.
6. **Publish the feed with its freshness state visible** to every consumer, so a downstream agent can fail closed on staleness.
7. **Never interpolate, extrapolate, or estimate a missing figure into the feed.**

## Output

- A market feed by metro, county, ZIP, and neighborhood, every figure carrying source and as-of date.
- An explicit freshness state per measure, published to every consumer.
- Named source disagreements and named coverage gaps.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VANTAGE — these apply to every VANTAGE skill, per `AGENTS.md` §5:**

- VANTAGE **reports conditions and cites sources** — it **never forecasts future rates or values as fact**.
- Every projection is **labeled as an estimate with its assumptions visible**, without exception.
- Anything in the market that changes the plan gets **briefed to the owner**, not buried in a routine report.

**Specific to this skill:**

- **Every figure carries its source and its as-of date, always.** A market number without provenance cannot be defended to a client, a regulator, or the person who acted on it, and VANTAGE's whole boundary rests on citing sources.
- **A missing figure is reported as missing.** VANTAGE never substitutes a coarser geography for a finer one, never interpolates between periods, and never estimates a value into the feed — an estimated figure that enters the feed unlabeled becomes a fact by the time it reaches a client report.
- **Freshness windows are per measure.** Rate data stale by a day and absorption data stale by a day are entirely different situations, and one window across all measures gets both wrong.
- **Stale is served as stale, or not served.** The freshness state is published to consumers precisely so `ember-trigger-feed` and LEDGER can fail closed on it, and a feed that hides its own staleness defeats every downstream guard at once.
- **Source disagreement is reported, never resolved by averaging.** Two sources disagreeing is information; a blended number is that information destroyed.
- **The feed reports conditions; it never draws a conclusion about where they are heading.** Direction is a projection, and projections belong to [`estimate-labeler`](../estimate-labeler/SKILL.md)'s rules.

## Measured on

Data freshness · figures served without source and as-of date (target zero) · coverage against the geographies the business operates in
