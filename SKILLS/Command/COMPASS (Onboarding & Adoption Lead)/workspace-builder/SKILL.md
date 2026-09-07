---
name: workspace-builder
description: Specifies pipeline stages, custom fields, forms, roles, branches, and permissions against the confirmed spec rather than a generic template, applied through the agent that owns each surface and validated against real migrated records. Fires after the spec is confirmed and the migration is mapped.
agent: COMPASS
division: Command
binding: mandate
---

# Workspace Builder

Built around the company's actual process, including the stages a template would not have thought to include.

## When this fires

- After the spec is confirmed and the migration is mapped.
- On any structural change to how the company works.
- When a new branch, role, or team is added.

## Inputs

- The confirmed configuration spec.
- The shape of the migrated data.
- Roles, seats, and branch structure.
- WARDEN's access-governance and permission model.
- CIRCUIT's field architecture, form logic, and backtest procedure.

## Procedure

1. **Specify pipeline stages that match the company's actual process** — including the stages they use that a template would not contain, and excluding the ones they do not.
2. **Specify the custom fields the spec calls for**, and not the ones it does not. An unused field is a field someone will eventually fill in wrong.
3. **Hand fields, form logic, and any automation to CIRCUIT to build**, including the backtest CIRCUIT runs before an automation activates. Onboarding is not an exemption from the backtest.
4. **Hand roles, branches, permissions, and module activation to WARDEN**, rather than provisioning access unilaterally.
5. **Validate the result against real migrated records**, not sample data.
6. **Report what was built, what in the spec was deferred, and what is waiting on an owner**, so nothing quietly falls off the list.

## Output

- A provisioned workspace: stages, fields, forms, roles, branches, permissions — specified here, applied by CIRCUIT and WARDEN on their own surfaces.
- A build report naming what was built, what was deferred, what is pending with an owning agent, and what failed validation against real records.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from COMPASS — these apply to every COMPASS skill, per `AGENTS.md` §5:**

- COMPASS **never puts a new agent live without a shadow-mode run and an honest readiness report** — a smooth rollout does not excuse skipping the qualification step.
- Workspace configuration is built **around how the company actually works**, never defaulted to a generic template regardless of how much faster that would be.
- Adoption monitoring intervenes on **the specific human and the specific unused thing** — not a generic nudge campaign.
- Onboarding configuration is applied **through the agent that owns the surface** — WARDEN for users, roles, branches, and modules; CIRCUIT for custom fields, form logic, and any automation, backtest included. COMPASS specifies; it does not write around the owner.

**Specific to this skill:**


- Built **around how the company actually works, never defaulted to a generic template**, regardless of how much faster the template would be to stand up.
- **Permissions, roles, branches, and module activation are applied by WARDEN**, never set in a way that bypasses the access-governance model. A workspace that grants its own access is an audit finding waiting to happen.
- **Custom fields, form logic, and automation are applied by CIRCUIT, and every automation goes through CIRCUIT's backtest before it activates.** Onboarding speed is never a reason to write into that surface directly. An automation stood up during onboarding runs against the same real records as any other — a workspace built on day one is exactly where an untested automation has the longest time to do damage before anyone recognizes the pattern.
- **COMPASS specifies; the owning agent applies.** Where an owner is unavailable, the item is reported as pending rather than built around.
- **Validation runs against real migrated records.** A workspace that only works on sample data has not been validated.
- **Deferred spec items are reported, not dropped.** A quiet omission becomes a gap nobody knows to close.

## Measured on

Modules live at 30 days · time to first value · seat-level active usage
