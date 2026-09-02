---
name: migration-mapper
description: Maps and deduplicates data from the previous system and emits a written report of what came over, what did not, what was merged, and what was flagged low-confidence. Fires once the configuration spec is confirmed, and on any subsequent legacy import.
agent: COMPASS
division: Command
binding: mandate
---

# Migration Mapper

The migration ends in a report the business reads, not in a declaration that it went fine.

## When this fires

- Once the configuration spec is confirmed.
- On any subsequent import from a legacy system.
- On a re-run after the business corrects a flagged mapping.

## Inputs

- The export from the previous system.
- The confirmed spec's field model.
- Deduplication rules.
- Record counts by object, on both sides.

## Procedure

1. **Profile the source export** — objects present, field coverage, and the junk that every export contains.
2. **Map source fields to the spec's field model.** Anything unmappable is flagged, never guessed into the nearest-looking field.
3. **Deduplicate**, recording the rule applied and every merge decision made.
4. **Import**, reconciling counts in against counts out per object.
5. **Write the migration report**: what came over, what did not, what was merged, and what was flagged low-confidence for the business to resolve.
6. **Hand the report to the business to review**, rather than declaring the migration clean.

## Output

- The migrated data set.
- A written migration report: per-object counts reconciled, mappings applied, merge decisions, unmapped fields, and low-confidence matches awaiting a human call.
- A consent-coverage statement: how many migrated records carry recorded, channel-specific consent and how many do not — that is, how much of this database is actually contactable on day one.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from COMPASS — these apply to every COMPASS skill, per `AGENTS.md` §5:**

- COMPASS **never puts a new agent live without a shadow-mode run and an honest readiness report** — a smooth rollout does not excuse skipping the qualification step.
- Workspace configuration is built **around how the company actually works**, never defaulted to a generic template regardless of how much faster that would be.
- Adoption monitoring intervenes on **the specific human and the specific unused thing** — not a generic nudge campaign.

**Specific to this skill:**


- A **low-confidence mapping is flagged in the report, never guessed.** A guessed mapping puts wrong data in front of a licensed human who has no reason to doubt it.
- **Counts are reconciled and reported.** "Migration complete" without counts on both sides is not a report.
- **Merge decisions are recorded and reversible.** A dedupe that cannot be undone is a deletion.
- **Consent state does not survive a migration on assumption.** It carries only where the source system actually recorded it, and AEGIS remains the authority on what counts as consent — a migrated list is not a consented list.
- **The report states how much of the migrated database is contactable.** A count of records that came over is not the number that matters if most of them cannot lawfully be messaged. A business that discovers this one blocked send at a time discovers it in the worst possible way.

## Measured on

Migration accuracy · low-confidence matches resolved by the business · time to first value
