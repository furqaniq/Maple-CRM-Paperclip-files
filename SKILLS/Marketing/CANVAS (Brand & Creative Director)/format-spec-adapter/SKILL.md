---
name: format-spec-adapter
description: Ships one concept correctly sized to every placement from maintained per-channel format specs. Fires when an approved concept needs more than one placement, when a platform changes its spec, and when a new placement is connected.
agent: CANVAS
division: Marketing
binding: mandate
---

# Format Spec Adapter

A disclosure cropped out of a story format is a missing disclosure, not a formatting artifact.

## When this fires

- An approved concept needs to ship to more than one placement.
- A platform changes its format spec, safe areas, or text limits.
- A new placement or channel is connected to the account.
- An asset scheduled against a superseded spec has not yet gone out.

## Inputs

- The approved concept and its master composition.
- The maintained per-channel spec table: dimensions, safe areas, file size, duration, text limits, compression behavior.
- Every destination placement the concept is going to.
- The disclosure block and the space it requires.
- Each platform's crop and truncation behavior, including how previews and thumbnails render.

## Procedure

1. **Read the spec for each destination** before adapting anything — dimensions, safe area, file size, duration, and text limits differ per placement, not per platform.
2. **Re-compose per placement rather than scaling one master.** A crop is not an adaptation; it is the master with parts of it missing, and the missing parts are rarely random.
3. **Verify the disclosure survives every crop, safe area, and text limit** in the actual rendered output — including the preview, the thumbnail, and the first frame where those are what most viewers see.
4. **Check legibility at the platform's real rendered size**, after the platform's own compression, not in the design file.
5. **Re-submit each output to brand review.** Adaptation is production, not a pass-through of an already-approved thing.
6. **Maintain the spec table** when a platform changes, and re-check every asset already scheduled against the old spec rather than only future ones.

## Output

One composed output per destination placement, each verified to carry its disclosure legibly at rendered size after platform compression, each independently brand-reviewed — and a re-check pass over scheduled assets whenever a spec changes.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CANVAS — these apply to every CANVAS skill, per `AGENTS.md` §5:**

- CANVAS **rejects off-brand work regardless of which agent produced it.** There is no exception for internal urgency, for a campaign already scheduled, or for an asset ATLAS dispatched.
- Co-branded variants are **always populated live from profile records** — credentials are never hardcoded into an asset.
- CANVAS **blocks imagery direction that implies a preferred demographic** at generation time, not at review.

**Specific to this skill:**

- **A disclosure lost to a crop, a safe area, a text limit, or a compression pass is a missing disclosure.** The placement is dropped; the disclosure is not. A concept ships to the placements where it fits fully and does not ship to the ones where it does not.
- **Adaptation never degrades into cross-posting an identical file.** Each placement gets its own composed output, or it gets nothing — and "nothing" is reported as a gap rather than filled with the master.
- **A spec change re-checks assets already scheduled**, not only assets yet to be made. The scheduled ones are the ones that will go out wrong first.
- **Legibility is judged at the platform's rendered size after its compression**, never in the design file, where everything is legible and nothing is proven.
- **CANVAS produces adaptations; BEACON specifies what a platform needs.** The platform requirement is BEACON's to state and CANVAS's to satisfy. An adaptation authored anywhere else is an ungated asset, and a platform need CANVAS invented is a guess about a surface it does not operate.
- Each adapted output is reviewed on its own. Clearing the master clears nothing downstream of it.

## Measured on

Brand consistency across published assets · production volume · asset reuse rate
