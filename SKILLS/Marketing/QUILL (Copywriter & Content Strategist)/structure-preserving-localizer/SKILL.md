---
name: structure-preserving-localizer
description: Translates and localizes without losing the persuasive structure of the original, and without ever machine-translating a required disclosure. Fires when an asset must ship in another language or locale, and when a source asset changes and its localized versions fall behind.
agent: QUILL
division: Marketing
binding: mandate
---

# Structure-Preserving Localizer

A translated disclosure is an unapproved disclosure. Everything else about localization follows from that.

## When this fires

- An asset must ship in a language or locale other than the one it was written in.
- A contact or segment has a stated language preference the account serves.
- A source asset changes, leaving its localized versions carrying an older message.
- A locale's requirements change.

## Inputs

- The source asset and its stated persuasive structure.
- The target language and locale, and the jurisdiction that governs there.
- The brand voice as CANVAS defines it in the target language — voice does not survive translation unattended.
- **The approved-language disclosure versions** held in the profile and AEGIS's disclosure builder.
- The target channel's limits, which the source's limits do not predict.
- The link between source and localized versions.

## Procedure

1. **Identify the persuasive structure of the source** — the claim, its support, the objection handled, the ask, and the order they arrive in. That structure is what is being preserved, not the sentences.
2. **Translate meaning and structure, not words.** An idiom rendered literally loses the argument it was carrying.
3. **Handle the disclosure block separately and never translate it.** It is replaced with the approved-language version from the profile or AEGIS's disclosure builder. If no approved version exists in that language, the asset does not ship in that language.
4. **Re-run [`generation-time-compliance-filter`](../generation-time-compliance-filter/SKILL.md) on the translated output.** A construction that is neutral in one language is coded in another, and the source's pass does not transfer.
5. **Re-check the channel limits in the target** — translations expand, and an SMS that fit in the source will not fit here.
6. **Verify the brand voice survived** in the target, against CANVAS's definition for that language.
7. **Link the localized asset to its source version**, so a source change flags every localization rather than leaving them quietly behind.

## Output

A localized asset preserving the source's persuasive structure, carrying the approved-language disclosure rather than a translated one, independently screened in the target language, fitted to the target channel's limits, and linked to the source version it derives from.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **Required disclosure text is never machine-translated.** It is served from the approved-language version, or the asset does not ship in that language. A translated disclosure has not been approved by anyone and is not a disclosure.
- **The compliance filter re-runs on the translated output.** A source pass carries nothing across a language boundary — the proxy list, the steering patterns, and the coded phrasing are all language-specific.
- **Language is never used as a targeting proxy.** Producing a variant in a language to reach a national-origin group is steering; producing one because contacts have stated a language preference is service. The difference is which direction the decision ran, and it is recorded.
- **A claim that cannot be made compliantly in the target locale stops the asset there.** It is not softened into a weaker claim the source never made — that is a different asset with no evidence behind it.
- **A localized asset out of date with its source is withdrawn, not left standing.** Two live versions of the same message saying different things is worse in a second language than in the first, because nobody monitoring the first will notice.

## Measured on

Compliance rejection rate under 2% · turnaround time · engagement lift per variant
