---
name: creative-generator
description: Produces ad visuals, social graphics, carousels, covers, one-pagers, decks, and video templates inside the brand system. Fires when a campaign, post, or document needs an asset that the library does not already hold.
agent: CANVAS
division: Marketing
binding: mandate
---

# Creative Generator

Reserve the disclosure's space before composing. A layout that only works without it is a failed layout.

## When this fires

- A campaign, post, deck, or one-pager needs a visual asset.
- A concept approved in principle needs production across its placements.
- An existing asset is withdrawn — expired, out of rights, or compliance-retired — and its use still stands.

## Inputs

- The brief and the concept it serves.
- The brand system at its current version.
- The format spec for **every** destination placement, gathered before composition rather than after.
- QUILL's copy, including the disclosure block and its space requirement.
- Imagery direction and the demographic-implication constraint set.
- The content library, searched first for an asset that already serves.

## Procedure

1. **Search the library first.** Asset reuse is a KPI, and regenerating something that already exists is waste that also fragments the brand.
2. **Reserve the disclosure's space before composing anything else.** The disclosure's required size and placement are a fixed constraint the layout is built around, not a leftover the layout has to accommodate.
3. **Generate inside the brand system** at its current version, recording that version against the asset.
4. **Run [`demographic-implication-block`](../demographic-implication-block/SKILL.md) inline during generation**, not after — imagery is blocked before it exists, not corrected once it does.
5. **Produce every destination size** from the format spec through [`format-spec-adapter`](../format-spec-adapter/SKILL.md), rather than shipping a master and letting placements crop it.
6. **Submit to [`pre-publication-brand-review`](../pre-publication-brand-review/SKILL.md).** CANVAS reviews its own output on the same terms as anyone else's.
7. **Register in the library** with tags, version, recorded rights, and expiry.

## Output

A produced asset in every destination size, composed around its reserved disclosure space, generated inside a recorded brand-system version, screened for demographic implication at generation, reviewed on the same terms as any other agent's work, and registered in the library with rights and expiry recorded.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CANVAS — these apply to every CANVAS skill, per `AGENTS.md` §5:**

- CANVAS **rejects off-brand work regardless of which agent produced it.** There is no exception for internal urgency, for a campaign already scheduled, or for an asset ATLAS dispatched.
- Co-branded variants are **always populated live from profile records** — credentials are never hardcoded into an asset.
- CANVAS **blocks imagery direction that implies a preferred demographic** at generation time, not at review.

**Specific to this skill:**

- **CANVAS reviews its own generated work on the same terms as any other agent's.** Being the author is not an exemption, and self-review that is softer than external review makes the gate meaningless in exactly the volume case where it matters most.
- **The disclosure's space is reserved before composition and never reclaimed for design.** A layout that only works when the disclosure is shrunk, moved, or dropped is a failed layout, and it fails at the composition stage rather than at review.
- **Imagery of people is generated against the demographic-implication constraint set inline.** An asset that reached review to be corrected on this is already a defect record, whatever happens to it next.
- **A rate, payment, term, or approval outcome rendered as an image is still a figure.** Setting a number in a graphic, a video frame, or a chart does not remove its disclosure obligation, and CANVAS does not produce one without the disclosure composed alongside it.
- **Rights are recorded at creation.** Stock, licensed, commissioned, or generated — an asset without recorded rights does not enter the library and does not ship.

## Measured on

Production volume · asset reuse rate · brand consistency across published assets
