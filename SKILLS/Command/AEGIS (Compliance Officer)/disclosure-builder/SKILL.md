---
name: disclosure-builder
description: Assembles required disclosures live from the sending user's profile — license identifiers, equal-opportunity notice, jurisdictional limits, AI disclosure, recording notice — so nothing ships stale or incomplete. Fires at generation time on every outbound asset.
agent: AEGIS
division: Command
binding: mandate
---

# Disclosure Builder

Disclosures are built at send time from live profile data, because a copied disclosure is a disclosure that will eventually be wrong.

## When this fires

- At generation time, on every outbound asset on every channel.
- When a sending user's licensure or profile changes — everything queued under the old profile is rebuilt.
- When a send crosses into a jurisdiction the previous build did not account for.

## Inputs

- The sending user's live profile: license identifiers, the jurisdictions they are licensed in, entity details.
- The channel and asset type.
- The contact's jurisdiction.
- The required-disclosure matrix for that channel, asset, and jurisdiction pair.

## Procedure

1. **Identify the sending user and the contact's jurisdiction**, and confirm the user is licensed there.
2. **Pull license identifiers live** from the profile — never from a cached copy, a template, or a previous send.
3. **Select the required set** for this channel, asset type, and jurisdiction: license identifiers, equal-opportunity notice, jurisdictional limits, AI disclosure, recording notice.
4. **Assemble and place** them so they survive the channel's own rendering and truncation behavior.
5. **Verify completeness.** If a channel's limits would truncate a required disclosure, block the send rather than trim it.
6. **Log the exact disclosure set** that shipped, so the audit ledger can reconstruct what the consumer actually saw.

## Output

- The assembled disclosure block, placed for the channel.
- A block verdict where the user is unlicensed in the jurisdiction or the disclosure cannot fit intact.
- An audit entry recording the exact disclosures shipped and the profile version they were built from.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from AEGIS — these apply to every AEGIS skill, per `AGENTS.md` §5:**

- AEGIS inspects **100% of outbound communication** — a deterministic rule pass first, a judgment pass second, never judgment alone.
- AEGIS **reports to the Account Owner, never to ATLAS**, and its blocks **cannot be overridden by an admin, by ATLAS, or by a plan upgrade** — no exception, regardless of who asks.
- AEGIS **cannot be disabled**.
- Opt-outs are honored **instantly and irreversibly**.

**Specific to this skill:**


- Disclosures are **built live at send time**. A cached, templated, or hand-copied disclosure is a stale disclosure, and staleness here is the failure mode.
- A **channel's character limit is never a reason to trim a required disclosure.** The message shortens; the disclosure does not. If it still will not fit, the send is blocked.
- A user **unlicensed in the contact's jurisdiction does not get a best-effort disclosure** — the send is blocked outright.
- **AI disclosure and recording notice are never omitted** because they reduce response rate. Response rate is not a compliance input.
- Queued content built from a now-changed profile is **rebuilt, not released** on the old version.

## Measured on

Disclosure completeness (target 100%) · sends blocked for stale or missing disclosure · gate latency
