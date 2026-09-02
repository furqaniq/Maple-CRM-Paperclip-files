---
name: template-library-curator
description: Builds the template library and retires what underperforms or has gone non-compliant, instead of letting it accumulate. Fires on the scheduled library review, on a settled performance result, and immediately on any compliance invalidation.
agent: QUILL
division: Marketing
binding: mandate
---

# Template Library Curator

A library nobody prunes is a liability with a search box.

## When this fires

- On the scheduled library review cycle.
- When a template's performance crosses the retirement threshold on a **settled** result.
- Immediately, when a template is invalidated on compliance grounds — an AEGIS block, a stale disclosure, a profile or regulatory change.
- When a new asset proves durable enough to become a template.

## Inputs

- The template library with per-template version history, usage counts, and last-used dates.
- Performance results from LEDGER, and test outcomes from RELAY with their significance stated.
- AEGIS block and rejection records, attributed to the template that produced the asset.
- Profile and regulatory changes affecting disclosure requirements.
- Active and scheduled campaigns, and which templates they reference.

## Procedure

1. **Review usage and performance** per template against the retirement threshold.
2. **Retire underperformers on a settled result only.** A test RELAY called inconclusive retires nothing.
3. **Retire on compliance grounds immediately**, independent of performance and independent of the review cycle. A template AEGIS has blocked, or whose disclosure has gone stale, is withdrawn on detection.
4. **Withdraw rather than delete.** A retired template stops being servable to new work and stays readable in version history.
5. **Find every live reference first.** Before a withdrawal takes effect, identify the active and scheduled campaigns using the template and tell their owners, naming the replacement.
6. **Record the reason** — performance, compliance, or supersession — against the template, so the same question is not reopened next cycle.
7. **Promote proven assets** into the library with their tags, channel, and the hypothesis history that earned them the slot.

## Output

An updated library: templates promoted with their evidence, templates withdrawn with their reason and their version history intact, and a notice to every agent holding a live reference to a withdrawn template, naming its replacement.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **Compliance retirement is immediate and never waits.** A template that is compliant but underperforming can wait for the cycle; a template that is performing but non-compliant cannot, and performance is never weighed against it.
- **Retirement never deletes.** Version history is an audit record of what the company said and when, and it survives withdrawal.
- **A template in flight is never silently swapped.** RELAY and BEACON are told before the send, not after — a campaign that changed message mid-flight without its owner knowing is worse than one that paused.
- **Never retires on an inconclusive result.** Acting on noise is the failure RELAY's threshold exists to prevent, and QUILL does not reintroduce it downstream.
- A template's compliance rejection record belongs to the template, not to the run that used it. Repeated rejections mean the template is wrong even when each individual asset was fixed.

## Measured on

Template library performance · compliance rejection rate under 2%
