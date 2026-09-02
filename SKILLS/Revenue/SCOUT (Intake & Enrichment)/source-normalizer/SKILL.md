---
name: source-normalizer
description: Maps every inbound source — paid social, search, landing pages, portals, referral partners, inbound calls, spreadsheets, and API — into one canonical record shape. Fires on every arriving lead and on every new or changed source connection.
agent: SCOUT
division: Revenue
binding: mandate
---

# Source Normalizer

Every downstream agent reads one record shape. The variety lives here and stops here.

## When this fires

- On every lead arriving from any connected source.
- On a referral handed over by EMBER's `record-grounded-referral-ask`, which carries its lineage and arrives with no consent of its own.
- When a new source is connected, before it is allowed to write records.
- When a source changes its payload shape and a mapping stops resolving.
- On a bulk spreadsheet or API import.

## Inputs

- The raw payload exactly as the source delivered it.
- The source's mapping definition and its version.
- The canonical record schema, as CIRCUIT's `field-architecture-keeper` holds it.
- Source lineage — which connection, which campaign, which form, at what time.

## Procedure

1. **Capture the raw payload unmodified** and keep it, so a mapping error is recoverable rather than a permanent loss.
2. **Apply the source's mapping** to produce the canonical record.
3. **Normalize formats deterministically** — phone, email, name, address, currency, date — so `identity-resolver` compares like with like rather than re-deriving the normalization itself.
4. **Attach source lineage to the record**, including the connection, campaign, form, and arrival timestamp.
5. **Route an unmapped or newly-shaped field to a flag, never to a guess.** A value dropped into an approximately-right field is worse than a value held aside.
6. **Hand the normalized record to [`identity-resolver`](../identity-resolver/SKILL.md)** before anything else touches it.

## Output

- One canonical record with source lineage attached.
- The retained raw payload, linked to the record it produced.
- A mapping-gap flag naming any field that arrived and could not be placed.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SCOUT — these apply to every SCOUT skill, per `AGENTS.md` §5:**

- SCOUT **never pulls, requests, infers, or stores consumer credit information**, under any circumstance.
- Position estimates are **always labeled as estimates** — never presented as fact.

**Specific to this skill:**

- **The raw payload is retained.** A normalizer that discards what it could not map makes its own errors undiagnosable, and a mapping bug found three weeks later is unrecoverable without the original.
- **An unmapped field is flagged, never guessed into an approximate field.** A value in the wrong field is read by every downstream agent as though it were in the right one.
- **Normalization never invents data.** A missing area code is not inferred from the source's geography, and a partial address is not completed from a market average.
- **Field creation goes through CIRCUIT's `field-architecture-keeper`**, not through a source mapping. A source that needs a new field is a schema request, not a local exception, or the data model forks by connection.
- **Source lineage is written at normalization and is never backfilled from inference.** LEDGER's `event-stream-attribution` holds spend by channel and campaign and rests entirely on this lineage; ABACUS meters the platform's own consumption and never held lead-source spend. A reconstructed lineage looks exactly like a recorded one.
- **No credit field is mapped, ever, regardless of what a source sends.** A source that delivers consumer credit information has that field dropped at the boundary and the delivery reported — see [`credit-data-firewall`](../credit-data-firewall/SKILL.md).

## Measured on

Fields normalized without a gap flag · mapping errors reaching downstream agents (target zero) · sources live with a complete mapping
