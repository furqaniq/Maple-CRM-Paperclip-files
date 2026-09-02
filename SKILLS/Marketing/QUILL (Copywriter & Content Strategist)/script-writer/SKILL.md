---
name: script-writer
description: Generates scripts for calls, video, and voicemail — written to be spoken, with the mandatory branch points written as speakable lines rather than stage directions. Fires when VOX or a human needs a script, and on a script refresh after a rule or profile change.
agent: QUILL
division: Marketing
binding: mandate
---

# Script Writer

A compliance requirement written as a stage direction is a compliance requirement nobody says out loud.

## When this fires

- VOX needs a script for a call type, or a human needs one for a live conversation.
- A video or voicemail script is requested.
- A jurisdictional rule changes — recording consent, required identification, quiet-hours language.
- A profile change alters the identification a script must speak.

## Inputs

- The purpose of the conversation and its intended outcome.
- The channel: a live two-way call, a one-way voicemail, or recorded video.
- Who speaks it — VOX, or a named human — because the disclosure obligations differ.
- Jurisdictional obligations: call-recording consent, caller identification, NMLS identification, solicitation language.
- The brand voice from CANVAS, rendered for speech.
- VOX's immediate-transfer boundary and the account's opt-out mechanics.

## Procedure

1. **Establish purpose and speaker** — an agent-spoken script and a human-spoken script are different documents with different obligations.
2. **Identify the spoken disclosure obligations** for the channel and jurisdiction: recording consent, who is calling, on whose behalf, and NMLS identification.
3. **Write it to be spoken** — short clauses, plain words, natural stress. Copy that reads well on a page and cannot be said aloud is a failed script.
4. **Write the mandatory branch points as speakable lines**, not as notes: the immediate-transfer path, the opt-out path, and the "I'm not able to answer that, here's who can" path.
5. **Run the compliance filter and attach disclosures** through live attachment, marking the profile-sourced values.
6. **Mark every line verbatim-required or improvisable.** Disclosure, identification, consent, transfer, and opt-out lines are verbatim.
7. **Register against the profile and rule set** it depends on, so a change reaches every script that speaks it.

## Output

A script marked line by line as verbatim-required or improvisable, carrying the spoken disclosure and identification lines, the immediate-transfer line, the opt-out line, and the decline-and-route line — registered against the profile fields and jurisdictional rules it consumes.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **Every script for a live conversation carries an immediate human-transfer line, speakable at any point.** It is never buried at the end, never conditional on the contact asking twice, and never phrased to discourage its use. VOX's boundary is absolute, and QUILL cannot write a script that erodes it.
- **Every script carries an opt-out path in plain language** — how to stop being contacted, said in words a person recognizes.
- **Disclosure, identification, and consent lines are marked verbatim-required** and never left to paraphrase or improvisation. A requirement the speaker restates in their own words is a requirement that eventually goes unmet.
- **No script contains an approval, denial, eligibility, or qualification statement**, including a softened or hypothetical one — "you'd probably be fine", "that usually qualifies", "I don't see a problem". These are the exact form the boundary exists to prevent.
- **Voicemail scripts identify the caller, the company, and the purpose on every message**, not only the first in a sequence.
- A script never instructs the speaker to continue after a stop request, a transfer request, or a question outside their boundary.

## Measured on

Compliance rejection rate under 2% · turnaround time · template library performance
