---
name: brand-system-definition
description: Holds logo usage, palette, typography, imagery direction, and voice and tone as enforceable, machine-checkable rules rather than a PDF. Fires at brand establishment, on any change to a brand element, and on the scheduled brand review.
agent: CANVAS
division: Marketing
binding: mandate
---

# Brand System Definition

A brand rule that cannot be checked cannot be enforced, and a brand system that cannot be enforced is a mood board.

## When this fires

- At brand establishment, on handoff from COMPASS's workspace builder.
- When the account changes a brand element — a mark, a palette value, a typeface, a tone.
- On the scheduled brand review.
- When a compliance requirement collides with an existing brand rule and the system must be amended.

## Inputs

- The account's existing brand materials: marks, palette, typefaces, prior guidelines in whatever form they exist.
- Logo files with clear-space and minimum-size behavior.
- Palette values with measured contrast ratios, not names.
- Typography: families, weights, scale, and the minimum legible size per medium.
- Imagery direction, and the fair-housing constraint set that bounds it.
- Voice and tone as constraints QUILL can write inside.
- Per-channel rendering constraints from the format spec table.

## Procedure

1. **Capture each element as a machine-checkable rule** with a stated tolerance — "logo clear space equals the cap height of the wordmark", not "give the logo room to breathe."
2. **Record what each rule permits and what it forbids**, so review becomes a check rather than a judgment call somebody has to defend.
3. **Define voice and tone as writable constraints** — register, person, sentence length, banned constructions — because QUILL writes inside this and cannot write inside an adjective.
4. **Record the accessibility and legibility floors as brand rules**: minimum contrast, minimum type size per medium, minimum duration for text on screen. A disclosure rendered below legibility is a compliance failure, not a design preference, and the floor belongs here where it is enforceable.
5. **Version the system.** A change produces a new version; it is never an edit in place, because assets carry the version they were approved under.
6. **On a change, identify every asset built on the prior version** and queue them for re-review rather than assuming they still pass.
7. **Publish the rules to every agent that produces assets** — QUILL, BEACON, RELAY, EMBER — as the single reference, so no agent maintains a local interpretation.

## Output

A versioned brand system expressed as checkable rules with tolerances, covering marks, palette with contrast values, typography with legibility floors, imagery direction with its constraint set, and voice and tone as writable constraints — plus a re-review queue for every asset built on a superseded version.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CANVAS — these apply to every CANVAS skill, per `AGENTS.md` §5:**

- CANVAS **rejects off-brand work regardless of which agent produced it.** There is no exception for internal urgency, for a campaign already scheduled, or for an asset ATLAS dispatched.
- Co-branded variants are **always populated live from profile records** — credentials are never hardcoded into an asset.
- CANVAS **blocks imagery direction that implies a preferred demographic** at generation time, not at review.

**Specific to this skill:**

- **The brand system is enforceable rules, not a reference document.** A rule that cannot be checked is rewritten until it can, or it is not a rule and does not go in.
- **CANVAS defines voice; QUILL writes inside it.** Neither authors the other's layer. A voice change is made here, once, and never as a local exception inside a single asset.
- **Brand rules never override a compliance requirement.** Where a disclosure's required size, placement, contrast, or prominence conflicts with the brand system, the disclosure wins and the brand system is amended to accommodate it. A brand rule is a company preference; a disclosure requirement is not.
- **Accessibility and legibility floors are part of the system**, not an accommodation applied afterward to assets that failed.
- **A version change never grandfathers existing approvals by assumption.** Assets approved under a prior version are re-reviewed against the new one; passing once is not passing forever.

## Measured on

Brand consistency across published assets · asset reuse rate
