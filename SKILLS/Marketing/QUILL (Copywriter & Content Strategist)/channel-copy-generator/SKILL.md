---
name: channel-copy-generator
description: Produces ads, emails, SMS, landing pages, and listing content in the brand voice CANVAS defines, at volume. Fires whenever another agent or a user needs copy for a specific channel and audience.
agent: QUILL
division: Marketing
binding: mandate
---

# Channel Copy Generator

The copy layer every channel-facing agent calls into instead of writing its own text.

## When this fires

- RELAY, BEACON, EMBER, or SCOUT needs copy for a send, a post, a sequence, or a reusable reply pattern for a conversational channel.
- A user asks for an asset for a named channel and audience.
- A template in the library needs a fresh instance populated for a specific campaign.
- An existing asset is invalidated — by a profile change, a brand-system version, or a compliance retirement — and needs regenerating.

## Inputs

- The brief: the outcome wanted, the audience, and the offer or message.
- The destination channel and its hard constraints — character limits, ad-platform policy, subject-line length, format.
- The brand voice, from CANVAS's brand system. QUILL writes inside it and does not set it.
- The contact or segment context available at generation.
- The user profile: NMLS ID, licensed states, entity, and every field a required disclosure draws on.
- The template library, for an existing template that already serves this brief.

## Procedure

1. **Resolve the channel** and load its hard constraints before writing a word — copy written to no limit and trimmed afterward is where disclosures get cut.
2. **Load the brand voice** from CANVAS's current brand-system version and write inside it.
3. **Check the library** for a template that already serves the brief rather than generating a near-duplicate.
4. **Draft** against the brief: one claim, its support, the objection it must survive, and the ask.
5. **Run [`generation-time-compliance-filter`](../generation-time-compliance-filter/SKILL.md) inline**, as part of writing, not as a review afterward.
6. **Attach disclosures** through [`live-disclosure-attach`](../live-disclosure-attach/SKILL.md), reading the profile at generation.
7. **Verify the finished asset still fits the channel with its disclosure intact.** If it does not, cut copy — never the disclosure — and if it still does not fit, stop and escalate to ATLAS.
8. **Emit** with the channel, the audience, the brand-system version, and the profile fields consumed recorded against it.

## Output

A channel-ready copy asset carrying its complete disclosure block, tagged with the destination channel, the audience, the brand-voice version it was written inside, and the profile fields it consumed — registered so a later profile change can find it.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **QUILL writes inside CANVAS's voice and never sets or overrides it.** A brief that needs the voice to bend is a request to CANVAS, resolved once in the brand system, never as a local exception inside one asset.
- **Passing QUILL's own filter is not compliance clearance.** Every outbound asset still passes the AEGIS gate. QUILL never marks an asset pre-approved, pre-cleared, or exempt, and never annotates it in a way a downstream agent could read as a reason to skip the gate.
- Copy never states or implies an approval, denial, eligibility, or qualification outcome — including hypothetically, and including on behalf of an agent whose own boundary forbids it.
- **A required disclosure is never trimmed to fit a channel limit.** The copy shrinks first; if the asset still will not fit, it does not ship on that channel and the constraint is escalated rather than resolved locally.
- An asset generated for internal review is generated to the same standard as one that ships. Internal drafts become outbound assets, and a draft exemption is the route by which uncompliant copy reaches a contact.

## Measured on

Turnaround time · compliance rejection rate under 2% · template library performance
