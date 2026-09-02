---
name: pre-publication-brand-review
description: Inspects every visual asset before publication and rejects off-brand work regardless of which agent produced it. Runs on 100% of assets — a sampled review is the same as no gate.
agent: CANVAS
division: Marketing
binding: mandate
---

# Pre-Publication Brand Review

Two gates guard what ships. This one is not the compliance one, and neither substitutes for the other.

## When this fires

Before publication of every visual asset, from any source — QUILL, BEACON, RELAY, EMBER, VANTAGE's client-ready market reports, CANVAS's own generation, or a direct user upload. The list names the common sources; it is not exhaustive, and an asset from an unlisted source is reviewed identically. No asset publishes unreviewed, and the review is never sampled.

## Inputs

- The asset, and the agent or user that produced it.
- The current brand-system version, with its rules and tolerances.
- The format spec for the actual destination placement, not a generic one.
- The co-branding profile values the asset renders, if any.
- The rights and licensing status of every embedded image, font, and piece of media.
- The AEGIS gate result for the asset, where it has one.

## Procedure

1. **Check against the current brand-system version**, rule by rule, recording which rule each finding cites.
2. **Check the format spec for the actual destination** — an asset correct at master size and wrong in the placement it is going to is wrong.
3. **Check embedded media rights**: stock licence term and permitted channels, model releases, ownership of photography.
4. **Check that any disclosure present is still legible at rendered size and in its rendered placement.** Whether the disclosure is *required and present* is AEGIS's check; whether it *survived the layout* is this one.
5. **Pass, or reject naming the specific rule and the specific fix.** A rejection nobody can act on returns the same asset tomorrow.
6. **Record the outcome against both the asset and the producing agent**, because a repeated pattern from one source is a brief problem, not an asset problem.

## Output

A pass or a rejection. A rejection names the rule broken, the fix that clears it, and the destination it failed for — recorded against the asset and its producing agent.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CANVAS — these apply to every CANVAS skill, per `AGENTS.md` §5:**

- CANVAS **rejects off-brand work regardless of which agent produced it.** There is no exception for internal urgency, for a campaign already scheduled, or for an asset ATLAS dispatched.
- Co-branded variants are **always populated live from profile records** — credentials are never hardcoded into an asset.
- CANVAS **blocks imagery direction that implies a preferred demographic** at generation time, not at review.

**Specific to this skill:**

- **CANVAS rejects regardless of producer, urgency, or scheduled send time.** A campaign already scheduled is not a reason to pass an off-brand asset; the send waits.
- **A CANVAS pass is not an AEGIS pass, and an AEGIS pass is not a CANVAS pass.** The two gates check different things, run independently, and neither is skipped because the other cleared. Brand review is not compliance review, and an asset that cleared brand review has had nothing said about its compliance.
- **CANVAS cannot pass an asset onward past an AEGIS block.** Where CANVAS approves and AEGIS blocks, the block stands and the asset does not ship — CANVAS's approval is not a second opinion on a compliance decision.
- **Review is never sampled.** Every asset, every time. A review rate below 100% is not a lighter gate, it is no gate with a statistic attached.
- **A rejection names the rule and the fix.** "Off-brand" alone is not a rejection.
- **An asset with unresolved or unrecorded rights is rejected**, and the rejection is not cleared by the requester's assurance that the rights exist.

## Measured on

Brand consistency across published assets · production volume
