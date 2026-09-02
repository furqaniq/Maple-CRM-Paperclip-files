---
name: discovery-interview
description: Runs the conversational business intake — model, team structure, sales process, tools being replaced — and turns the answers into a written configuration spec. Fires at the start of every new account's first thirty days and again for every new hire the company adds.
agent: COMPASS
division: Command
binding: mandate
---

# Discovery Interview

A conversation, not a form — and it ends in a spec the business has actually confirmed.

## When this fires

- At the start of every new account's first thirty days.
- Again for every new hire the company adds, since a seat is configured for a person and not just for a company.
- When the business model, team structure, or product mix changes materially enough that the existing configuration no longer fits.

## Inputs

- Conversation with the business.
- Team structure, roles, and how work actually moves between them.
- The sales process as run, which is frequently not the process as described.
- The tools being replaced, and what each one was genuinely used for.
- Volume and product mix.

## Procedure

1. **Interview conversationally.** A form gets the answers the business thinks it should give; a conversation gets the ones that are true.
2. **Cover the four areas**: business model, team structure and roles, the sales process as actually run, and the tools being replaced along with what each was used for.
3. **Probe the disagreements** — where the described process and the exported data do not match, the gap is the most useful thing in the interview.
4. **Turn the answers into a written configuration spec**: stages, fields, forms, roles, branches, permissions.
5. **Confirm the spec with the business** before anything is built against it.

## Output

A written configuration spec — pipeline stages, custom fields, forms, roles, branches, permissions — traceable to specific answers, and confirmed by the business before build.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from COMPASS — these apply to every COMPASS skill, per `AGENTS.md` §5:**

- COMPASS **never puts a new agent live without a shadow-mode run and an honest readiness report** — a smooth rollout does not excuse skipping the qualification step.
- Workspace configuration is built **around how the company actually works**, never defaulted to a generic template regardless of how much faster that would be.
- Adoption monitoring intervenes on **the specific human and the specific unused thing** — not a generic nudge campaign.

**Specific to this skill:**


- The spec reflects **how this company actually works.** A generic template is never the answer, no matter how much faster it would be to stand up.
- Where the **interview and the migrated data disagree, the disagreement is raised**, not silently resolved in favor of whichever is more convenient.
- **No workspace is built from an unconfirmed spec.** Building first and confirming later means the business reviews a thing it now has to unwind.

## Measured on

Time to first value · modules live at 30 days · 90-day retention
