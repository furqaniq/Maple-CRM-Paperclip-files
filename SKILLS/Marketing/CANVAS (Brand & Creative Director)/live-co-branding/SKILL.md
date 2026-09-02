---
name: live-co-branding
description: Populates per-user asset variants from profile records at render time, so credentials are never hardcoded and never wrong. Fires on every per-user render, and again on any profile or licensing change that invalidates variants already out there.
agent: CANVAS
division: Marketing
binding: mandate
---

# Live Co-Branding

A correctly printed NMLS ID on an ad running where that person is not licensed is still a violation.

## When this fires

- Any per-user variant of an asset is rendered.
- A profile record changes — name, title, NMLS ID, licensed states, entity, contact, headshot.
- A user's licensing status changes: lapses, is renewed, or adds or drops a state.
- A co-branded asset already scheduled or published is affected by either.

## Inputs

- The base asset and the fields it renders.
- The user's profile record: legal name, title, NMLS ID, licensed states, entity, contact, headshot.
- The **target audience's jurisdiction** for the placement.
- The current validity status of the user's licence in that jurisdiction.
- The brand system, applied to the rendered result rather than the template.
- The register of previously rendered variants and the profile fields each consumed.

## Procedure

1. **Render credentials live from the profile at render time** — never at design time, never baked into a stored file, never carried over from the last render.
2. **Verify the user's licensed states cover the audience's jurisdiction before rendering.** An ad correctly carrying a real NMLS ID, running where that person is not licensed, is a licensing violation dressed as a correct asset.
3. **Verify every required field is populated and current.** A missing or unverifiable field stops the render.
4. **Apply the brand system** to the rendered result, since a populated variant can break rules the empty template did not.
5. **Register the rendered variant** against the profile fields it consumed and the jurisdiction it was cleared for.
6. **On a profile or licensing change, invalidate every registered variant**: regenerate the unshipped, hold the scheduled, and withdraw or correct the published — and report the holds and withdrawals to ATLAS rather than handling them silently.

## Output

A per-user variant rendered live from the profile record, cleared against the licensing footprint for its placement's jurisdiction, brand-checked as rendered, and registered against the fields and jurisdiction it depends on — plus, on a change, an invalidation pass across unshipped, scheduled, and published variants.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CANVAS — these apply to every CANVAS skill, per `AGENTS.md` §5:**

- CANVAS **rejects off-brand work regardless of which agent produced it.** There is no exception for internal urgency, for a campaign already scheduled, or for an asset ATLAS dispatched.
- Co-branded variants are **always populated live from profile records** — credentials are never hardcoded into an asset.
- CANVAS **blocks imagery direction that implies a preferred demographic** at generation time, not at review.

**Specific to this skill:**

- **Credentials are never hardcoded, never cached past the render, and never borrowed.** A gap in one user's record is never filled from another user's, from the company record, or from a previous version of their own.
- **A missing or unverifiable credential stops the render.** No placeholder, no sample value, and no company-level NMLS ID standing in for an individual's — the individual identification requirement is not satisfied by an entity's.
- **A lapsed or out-of-jurisdiction licence blocks the render**, regardless of who requested it, what is scheduled against it, or how close the renewal is. This is not a brand rule and it is not waivable by the Account Owner.
- **A published co-branded asset whose credentials have gone invalid is withdrawn or corrected on detection**, and the withdrawal is reported to ATLAS with what was taken down — not resolved quietly as routine housekeeping.
- **Co-branding renders what the record asserts and never infers.** CANVAS does not add a title, a designation, a licence, or a credential the profile does not hold, however obviously it applies.

## Measured on

Brand consistency across published assets · production volume
