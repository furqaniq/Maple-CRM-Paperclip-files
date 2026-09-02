---
name: demographic-implication-block
description: Blocks imagery direction that implies a preferred demographic, at generation time rather than at review. Screens the brief, the imagery, the campaign as a set, and the pairing of imagery with targeting.
agent: CANVAS
division: Marketing
binding: mandate
---

# Demographic-Implication Block

Neutral imagery aimed at a demographically selected audience is one violation assembled from two compliant halves.

## When this fires

Inline, during any imagery generation or selection; when imagery direction is being set for a campaign; and on any brief that specifies who should appear in an asset. Never as a review step after the asset exists.

## Inputs

- The brief's imagery direction, in the words it was written in.
- The generated or selected imagery.
- **The full set of assets in the campaign**, not the single asset in hand.
- The placement's targeting parameters.
- The fair-housing constraint set, and the setting and signifier patterns that carry demographic meaning without depicting people.

## Procedure

1. **Screen the brief first.** A direction that names or implies a preferred demographic is blocked before generation starts — the asset is never made, so there is nothing to correct.
2. **Screen the imagery for who is depicted**, and how.
3. **Screen the campaign as a set.** Assets that are individually neutral can, together, depict one demographic — the set is the unit a regulator sees, and it is the unit judged here.
4. **Screen the pairing of imagery with targeting.** Neutral imagery aimed at a demographically selected audience achieves indirectly what neither half does alone.
5. **Screen setting and context cues** — a house of worship, cultural signifiers, a flag, a school, "the kind of neighborhood" — which carry the implication in assets with no people in them at all.
6. **Block, naming the specific implication**, and offer the direction that is not blocked so the work can proceed.
7. **Record the block against its brief source.** A repeated block on one brief template means the template is producing violations, not that reviewers are being strict.

## Output

A block or a clearance issued before the asset exists — a block naming the specific implication, the input that carried it (brief, imagery, set, or targeting pairing), and the direction that would clear; plus a defect record against the brief source on a repeat.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CANVAS — these apply to every CANVAS skill, per `AGENTS.md` §5:**

- CANVAS **rejects off-brand work regardless of which agent produced it.** There is no exception for internal urgency, for a campaign already scheduled, or for an asset ATLAS dispatched.
- Co-branded variants are **always populated live from profile records** — credentials are never hardcoded into an asset.
- CANVAS **blocks imagery direction that implies a preferred demographic** at generation time, not at review.

**Specific to this skill:**

- **The block applies at generation, before the asset exists.** An asset that reached review to be corrected on this is already a defect record, and the correction does not erase it.
- **The unit of judgment is the campaign, not the asset.** A set of individually unobjectionable assets that together depict a single demographic is blocked as a set, and no individual asset in it can be cleared on its own merits.
- **Imagery is never paired with demographic targeting.** The pairing is screened as one decision, because that is how it operates and how it will be read. Where the audience is a RELAY segment rather than a placement's targeting parameters, that segment's criteria are a required input to this screen — CANVAS cannot see half the decision and call it cleared, so a segment it cannot obtain blocks the screen rather than passing it.
- **A block is not overridable — not by the Account Owner, not by the requester, not by an urgency claim, and not by a plan tier.** CANVAS declines and escalates to ATLAS. It does not produce a marginal version that is arguably fine.
- **The absence of people is not automatically a pass.** Setting, signifiers, text, and the neighborhood a photograph was taken in can all carry the implication on their own.

## Measured on

Brand consistency across published assets · compliance rejection rate
