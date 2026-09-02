---
name: plain-language-documenter
description: Documents every workflow, integration, and webhook in plain language so the company is never hostage to whoever built it. Fires at build time, on every modification, and whenever documentation and actual behavior diverge.
agent: CIRCUIT
division: Operations
binding: mandate
---

# Plain-Language Documenter

The documentation is the deliverable that survives the person who built the workflow.

## When this fires

- At build time, for every workflow, integration, and webhook.
- Whenever a workflow is modified, structurally or otherwise.
- On the scheduled documentation review.
- When a workflow's documented behavior and its actual behavior have diverged.

## Inputs

- The workflow definition as it currently runs.
- Its trigger, branches, conditions, delays, and actions.
- Every field it reads and every field it writes.
- Its backtest report, modification history, and named owner.

## Procedure

1. **Describe the trigger in the terms the business uses**, not the terms the system uses.
2. **Describe each branch as the situation it handles and the outcome it produces.**
3. **Name every field read and every field written.**
4. **Name every outbound step and the gate it passes through.**
5. **State what the workflow deliberately does not do**, and what a reasonable reader might assume it does.
6. **Re-derive from the definition on every modification**, and flag where the previous description no longer matches what runs.
7. **Keep it readable by someone who did not build it and cannot read the definition.**

## Output

- A plain-language document per workflow, versioned alongside the definition.
- A named trigger, branch set, field-read and field-write list, and outbound-step list.
- An explicit statement of the workflow's non-behaviors.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from CIRCUIT — these apply to every CIRCUIT skill, per `AGENTS.md` §5:**

- No workflow activates **without a backtest against historical data** first.
- A detected silent failure, infinite loop, or conflicting trigger is **surfaced immediately** — never left running unflagged to "see if it resolves."
- Every workflow is **documented in plain language** — the company is never left hostage to undocumented automation.

**Specific to this skill:**

- **A workflow without current documentation is treated as undocumented, and undocumented automation is a finding.** Documentation describing a previous version is worse than none, because it is trusted and wrong.
- **Documentation is derived from the definition, never from the request that produced it.** What was asked for and what was built diverge; the document has to describe what actually runs.
- **The document names what the workflow does not do.** Most operational surprises come from an assumed behavior nobody ever wrote down as absent.
- Documentation is written for a **reader who cannot read the definition** — no internal identifiers, no node names, no system vocabulary standing in for a business meaning.
- **Documentation never contains a credential, a token, or contact-level data.** It describes the shape of what moves, never the contents.
- **A modification that cannot be described in plain language is a modification that is not understood**, and it is raised as such rather than documented vaguely.

## Measured on

Workflows with current documentation (target 100%) · documentation drift findings · handover time to a new owner
