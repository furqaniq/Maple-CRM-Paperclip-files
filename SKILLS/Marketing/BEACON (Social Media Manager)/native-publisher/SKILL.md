---
name: native-publisher
description: Publishes to every connected platform in native format at platform-specific optimal times, after verifying both the CANVAS pass and the AEGIS pass. Fires at each scheduled slot and on an asset becoming publishable.
agent: BEACON
division: Marketing
binding: mandate
---

# Native Publisher

A social post is outbound. It leaves the platform to people outside the account, so it goes through the gate like anything else.

## When this fires

- At each scheduled calendar slot.
- When an asset clears every gate that applies to it and becomes publishable into a waiting slot.
- At a platform's optimal time for the audience, within the slot's window.
- On a retry, after a publish failure inside its retry bound.

## Inputs

- The platform-native adaptation of the asset, from CANVAS's format spec adapter.
- The copy, from QUILL, in the form it cleared in.
- **The AEGIS gate result, and the CANVAS brand-review result where the asset has a visual component** — separate verifications, neither inferred from the other.
- Platform credentials and API state.
- Platform optimal-time data for this audience.
- Disclosure requirements, which differ between organic and paid placement.

## Procedure

1. **Verify every gate that applies to this asset has passed.** The AEGIS gate applies to every post without exception. CANVAS's brand review applies to every asset carrying a visual component. BEACON determines which gates apply and verifies each one it finds — it never assumes both and never infers one from the other.
2. **Verify this is the platform-native adaptation**, not the master and not another platform's output.
3. **Publish natively through the platform's own API**, never through a relay that strips formatting or reflows the caption.
4. **Publish at the platform's optimal time** for this audience, inside the slot's window.
5. **Confirm the publication landed** and record the live URL. A queued post is not a published post, and an API acceptance is not an appearance.
6. **On failure, retry within bound, then escalate.** A slot that failed is reported, never silently skipped.

## Output

A published post confirmed live with its URL recorded, the gate results it cleared on, the adaptation it used, and the time it published — or a reported publish failure naming the slot, the platform, and the reason.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from BEACON — these apply to every BEACON skill, per `AGENTS.md` §5:**

- Genuine leads surfacing in the social inbox are **routed into the CRM**, never left to die inside the platform.
- Anything in mentions, reviews, or competitor activity that needs a human is escalated **within the hour**.
- Reporting is always in **leads and pipeline**, never vanity metrics.

**Specific to this skill:**

- **A social post is outbound content.** It leaves the platform to parties outside the account, so it passes the AEGIS gate like any other outbound, on every channel, organic or paid.
- **Quiet hours and frequency caps are per-contact rules and do not apply to a broadcast post**, which has no recipient. They apply in full to every direct message BEACON sends. Applying a per-contact rule to a broadcast silently kills the evening posting window; failing to apply it to a direct message violates the contact.
- **A post publishes only when every gate that applies to it has passed, and never on one gate standing in for another.** The AEGIS gate applies to every post. CANVAS's review applies to every asset with a visual component — a text-only post is not held waiting for a pass that was never required, and is equally never stripped to text to avoid a review its visual would have needed. A CANVAS pass says nothing about compliance and an AEGIS pass says nothing about brand; neither is inferred from the other's presence.
- **Identical content is never cross-posted.** Each platform gets its own adaptation or gets nothing, and "gets nothing" is flagged to the calendar as an unfilled slot.
- **BEACON never edits copy or visuals at publish time** — not to fit a limit, not to fix a typo, not to improve a hook. An edit at publish is an ungated change to content that its gates already cleared in a different form. It goes back to QUILL or CANVAS, and the slot waits.
- **A post that failed to publish is reported.** A calendar showing a filled slot and a platform showing nothing is the exact failure this rule exists to prevent.

## Measured on

Publishing consistency · engagement rate
