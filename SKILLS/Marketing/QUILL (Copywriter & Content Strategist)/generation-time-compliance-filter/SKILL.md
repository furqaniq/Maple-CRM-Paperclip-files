---
name: generation-time-compliance-filter
description: Applies protected-class and steering constraints while writing, never through proxies, so no asset needs a compliance rewrite. Runs inline inside every QUILL generation — never as a separate review pass, and never disabled.
agent: QUILL
division: Marketing
binding: mandate
---

# Generation-Time Compliance Filter

An asset that reached review to be fixed was generated wrong. This is the skill that makes that true.

## When this fires

Inline, inside every QUILL generation, before the asset leaves the skill. Not a scheduled pass, not a review step, not a mode that can be switched off — including for drafts, internal assets, volume runs, and deadlines.

## Inputs

- The draft in progress.
- The protected-class constraint set: race, color, religion, national origin, sex, familial status, disability, and the credit-side additions — age, marital status, receipt of public assistance, exercise of a consumer-protection right.
- The steering-language pattern set.
- The proxy list — the correlated stand-ins that carry the same meaning without naming it.
- The channel and its targeting parameters, because content and targeting are screened together.

## Procedure

1. **Screen for direct reference** to a protected characteristic in the content or in the targeting the asset is written for.
2. **Screen for proxies** — the substitution failure mode, and the one that actually happens. Zip code and neighborhood name used as demographic stand-ins; school district standing in for composition; language preference; surname pattern; "traditional families"; religious or cultural signifiers; age-coded phrasing like "young professionals" or "starter"; "safe", "up-and-coming", "good schools" used as character rather than fact.
3. **Screen for steering** — any suggestion that a person or group belongs in, or does not belong in, a particular place, product, or price band.
4. **Screen for eligibility language** — approval, denial, qualification, or a rate presented as an outcome rather than an illustration.
5. **Rewrite at generation.** The asset that leaves this skill is the compliant one; nothing is flagged for someone else to fix.
6. **Record what was caught and rewritten** as a generation signal against the template or brief that produced it, because a repeated catch means the template is wrong, not the run.
7. **Refuse and escalate** a brief that cannot be satisfied compliantly, rather than writing the nearest permissible version of an impermissible request.

## Output

A screened asset with the violating constructions rewritten before the asset existed in shippable form, plus a defect record against the template or brief for anything caught — and, where the brief itself was the problem, a refusal escalated to ATLAS naming what cannot be written and why.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **The filter runs at generation, inline, and is never disabled** — not for speed, not for volume, not for a draft, not for an internal-only asset, not for a deadline. Internal drafts become outbound assets, and the draft exemption is the route by which uncompliant copy ships.
- **A proxy is treated exactly as the protected characteristic it stands in for.** The absence of a literal mention is not a pass, and "we never said it" is not a defense anyone has ever won with.
- **Passing this filter is not AEGIS clearance.** QUILL screens its own output so the gate has less to catch. The gate still inspects 100% of outbound, a QUILL pass never annotates an asset as pre-cleared, and no downstream agent may treat it as grounds to skip inspection. QUILL's rejection rate is a measure of how well this skill worked — not a substitute for the thing measuring it.
- **A brief that requires a violating asset is refused, not satisfied marginally.** QUILL does not write the compliant-looking version of a non-compliant request; it declines and escalates to ATLAS with the specific problem named.
- **Content and targeting are screened together.** Neutral copy aimed at a demographically-selected audience is the same violation assembled from two innocent halves, and screening only the words misses it entirely.

## Measured on

Compliance rejection rate under 2% · low-confidence actions taken unattended (target zero)
