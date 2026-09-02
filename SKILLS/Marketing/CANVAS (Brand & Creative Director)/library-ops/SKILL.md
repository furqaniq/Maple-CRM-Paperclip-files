---
name: library-ops
description: Runs the content library as a searchable versioned store — tagging, versioning, deduplication, expiry, and rights tracking. Fires on every asset entering the library, on the hygiene cycle, on an approaching rights or expiry date, and on a search that returns nothing.
agent: CANVAS
division: Marketing
binding: mandate
---

# Library Ops

Nothing is hard-deleted. What the company published is a record, and a record you can delete is not one.

## When this fires

- On every asset entering the library.
- On the scheduled hygiene cycle: duplicates, expiry, orphans, stale tags.
- When a rights term or an expiry date approaches, and again on the date itself.
- When a library search returns nothing — a miss is a production signal, not a user error.
- When QUILL retires a template that library assets are built around.
- On a brand-system version change, to queue every asset built on the superseded version for re-review rather than leaving the queue unread.

## Inputs

- Library contents with tags, versions, rights records, expiry dates, and usage counts.
- Brand-system versions, and which version each asset was approved under.
- Retirement signals from QUILL's template curator.
- Active and scheduled campaigns, and the assets each references.
- Search logs, including the queries that returned nothing.

## Procedure

1. **Tag on entry** — channel, pillar, campaign, brand-system version, rights, expiry. An asset entering untagged is unfindable, which is the same as absent.
2. **Version rather than overwrite.** A change produces a new version; prior versions stay readable because what was published is a record.
3. **Deduplicate onto a canonical asset**, repointing duplicates rather than removing them out from under a live reference.
4. **Expire on the recorded date**: an expired asset stops being servable to new uses, and every scheduled or published use of it is flagged to its owner.
5. **Track rights actively** — licence term, permitted channels, model releases, territory. An asset out of rights is withdrawn on the date, not at the next cycle.
6. **Treat search misses as a gap signal.** Repeated queries returning nothing tell CANVAS what to produce next, and they are reported rather than logged.
7. **Withdraw, never delete**, and record the reason against the version history.

## Output

A library where every asset is tagged, versioned, rights-recorded and expiry-dated; duplicates point at a canonical; withdrawn assets remain readable with their reason recorded; and a production gap list drawn from searches that found nothing.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CANVAS — these apply to every CANVAS skill, per `AGENTS.md` §5:**

- CANVAS **rejects off-brand work regardless of which agent produced it.** There is no exception for internal urgency, for a campaign already scheduled, or for an asset ATLAS dispatched.
- Co-branded variants are **always populated live from profile records** — credentials are never hardcoded into an asset.
- CANVAS **blocks imagery direction that implies a preferred demographic** at generation time, not at review.

**Specific to this skill:**

- **Nothing is hard-deleted.** Withdrawal removes an asset from use and leaves its record and version history readable, because what the company published — and when, and under whose credentials — is an audit trail.
- **An asset out of rights or past expiry is withdrawn immediately on detection**, including out of live campaigns, and the affected sends and posts are named to their owners. An expiry that fails silently is worse than no expiry date, because the date created the belief someone was watching.
- **Deduplication never removes an asset a live campaign references.** The reference is repointed first, and the removal follows.
- **An asset enters only with its rights recorded.** "Rights unknown" is a rejection, not a tag, and it is not resolved by the asset having been used before without incident.
- **Library state is never authority on compliance.** An asset present, current, and unexpired in the library has not thereby been cleared to send — CANVAS's review and the AEGIS gate are what clear it, every time it goes out.

## Measured on

Library search success · asset reuse rate · brand consistency across published assets
