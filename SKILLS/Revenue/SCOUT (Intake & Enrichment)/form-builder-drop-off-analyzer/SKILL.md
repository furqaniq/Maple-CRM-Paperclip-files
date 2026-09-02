---
name: form-builder-drop-off-analyzer
description: Builds intake forms with conditional logic and reports field-level abandonment so the field costing the most completions is known rather than guessed. Fires on a form build or change and on the drop-off reporting cycle.
agent: SCOUT
division: Revenue
binding: mandate
---

# Form Builder & Drop-Off Analyzer

Every field on an intake form has a conversion price, and the analyzer is how anyone finds out what it is.

## When this fires

- On a request to build or change an intake form.
- On the drop-off reporting cycle for every live form.
- When a form's completion rate moves materially against its own baseline.
- When a field on a form no longer maps to the canonical schema.

## Inputs

- The form's purpose, the audience it faces, and the fields requested.
- Field-level entry, completion, and abandonment events per session.
- The canonical schema from CIRCUIT's `field-architecture-keeper`, so every field lands somewhere real.
- AEGIS's protected-class constraints on what may be asked at all.

## Procedure

1. **Build the form against the canonical schema**, so no field collects data with nowhere to go.
2. **Screen every proposed field through AEGIS's `protected-class-screen` before the form goes live**, including free-text prompts that invite a protected characteristic as an answer.
3. **Use conditional logic to ask less**, revealing a field only when the prior answers make it relevant.
4. **Instrument each field for entry, completion, and abandonment.**
5. **Report abandonment at field level** — which field people reach and stop at, not the aggregate completion rate.
6. **Attach the conversion cost of a field to any request to add one**, so the trade is visible when the decision is made rather than after.
7. **Recommend removal or deferral of the field costing the most completions**, with the measured cost attached.

## Output

- A live form with conditional logic and field-level instrumentation.
- A field-level drop-off report naming the specific field costing completions, with its measured cost.
- A screening record for every field, showing what AEGIS passed.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SCOUT — these apply to every SCOUT skill, per `AGENTS.md` §5:**

- SCOUT **never pulls, requests, infers, or stores consumer credit information**, under any circumstance.
- Position estimates are **always labeled as estimates** — never presented as fact.

**Specific to this skill:**

- **No form collects a protected characteristic, or a field that functions as a proxy for one, in any framing.** This holds for optional fields, for free-text prompts, and for anything framed as a preference — an answer volunteered into a field the company chose to build is data the company chose to collect.
- **Drop-off analysis reads field-level events, never the content a visitor typed and abandoned.** Text entered into a form and never submitted was not given to the company, and reading it turns an analytics tool into surveillance.
- **No form collects consumer credit information, in any field, under any label** — see [`credit-data-firewall`](../credit-data-firewall/SKILL.md), including self-reported score ranges and pick-lists.
- **A field with no canonical destination is not built.** Data collected into nowhere is a promise to the visitor that their answer mattered.
- **A field's measured conversion cost is stated when it is requested, not only in the quarterly report.** The trade is only a decision if it is visible at the moment someone makes it.
- **Partial submissions are handled under the consent the visitor actually gave.** Reaching field four of six is not a request to be contacted, and treating an abandoned form as a lead is how a company earns a complaint it cannot defend.

## Measured on

Form completion rate · field-level drop-off identified and acted on · fields live without a canonical destination (target zero) · protected-class fields shipped (target zero)
