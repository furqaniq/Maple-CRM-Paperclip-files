---
name: live-disclosure-attach
description: Pulls required disclosures from the live user profile at generation, so nothing ships incomplete or goes stale when a profile changes. Fires on every asset carrying a figure, a solicitation, or a licensed identity — and again on every profile change, reaching assets already scheduled or published.
agent: QUILL
division: Marketing
binding: mandate
---

# Live Disclosure Attach

The disclosure problem is not attaching it once. It is what happens to every asset already out there when the profile changes.

## When this fires

- At generation, on every asset carrying a rate, payment, term, or cost figure; any solicitation; or any licensed identity.
- On any change to a profile field a disclosure draws on — NMLS ID, licensed states, entity name, title, contact.
- On a change to the requirement set itself, from AEGIS's disclosure builder.
- When an asset registered against a changed field is unshipped, scheduled, or already published.

## Inputs

- The user profile: NMLS ID, licensed states, entity legal name, title, contact, equal-housing and equal-opportunity obligations.
- The asset type, channel, and the jurisdiction of its audience.
- **AEGIS's disclosure-builder requirement set**, which is the authority on what is required for this combination.
- The asset register: every asset previously generated, and the profile fields each consumed.
- The state of each registered asset — unshipped, scheduled, or published.

## Procedure

1. **Determine the required set from AEGIS's disclosure builder**, keyed on asset type, channel, and audience jurisdiction. QUILL's job is attachment; determination is AEGIS's.
2. **Read the values live from the profile at generation** — never from a cached copy, never from a previous asset, never hardcoded into a template.
3. **Attach in full**, in the placement and prominence the channel requires.
4. **Register the asset against every profile field it consumed**, so a later change can find it rather than relying on someone remembering.
5. **On a profile or requirement change, re-resolve every registered asset in all three states**: unshipped assets regenerate; scheduled sends **hold** and regenerate before release; published assets are corrected or withdrawn.
6. **Report the hold.** A scheduled send stopped for re-resolution is declared to ATLAS with what is stopped and what will release it — a silent hold and a silent failure look identical from outside.
7. **Stop on a missing value.** A required profile field that is absent or unverifiable halts generation.

## Output

An asset carrying its complete disclosure block populated live from the profile, registered against the fields it consumed — plus, on a change, a re-resolution pass covering unshipped, scheduled, and published assets, and a declared hold on anything stopped mid-schedule.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **A disclosure is never abbreviated, truncated, restyled below legibility, or moved behind an interaction** to fit a layout or a character limit. The copy shrinks first. If it still does not fit, the asset does not ship on that channel — which is a channel decision, not a disclosure decision.
- **A missing profile value is a hard stop, not a placeholder.** QUILL never generates with a blank, an example value, a company-level fallback standing in for an individual's NMLS ID, or another user's credential.
- **AEGIS's disclosure builder defines what is required; QUILL attaches it.** Where the two differ, AEGIS is correct and QUILL's requirement set is the defect to be fixed — not a discrepancy to be split.
- **Attachment at generation never substitutes for AEGIS's verification at the gate.** This skill exists to make the gate's job small, not to replace it.
- **A profile change propagates to scheduled and published assets, not only to the next thing generated.** A scheduled send holds rather than going out stale, and the hold is visible and clocked. An asset already published under a now-invalid credential is corrected or withdrawn on detection.

## Measured on

Compliance rejection rate under 2% · turnaround time
