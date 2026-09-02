---
name: two-pass-gate
description: Runs the deterministic rule pass before the judgment pass on every outbound item, so a probabilistic system is never the last line of defense on a rule carrying statutory damages. Fires on 100% of outbound communication, before it leaves the platform.
agent: AEGIS
division: Command
binding: mandate
---

# Two-Pass Gate

Every outbound item clears a rules engine before a model ever sees it. The order is the whole point.

## When this fires

- On every outbound item, on every channel, before it leaves the platform.
- On regenerated or edited content — an edit re-enters the gate, it does not inherit the original's verdict.
- On content produced by any agent at any autonomy level, including L4.

## Inputs

- The outbound item: content, channel, sending user, recipient contact.
- The deterministic ruleset — consent state, quiet hours, DNC and suppression, required disclosures, hard-blocked phrasing.
- The judgment model, for what rules cannot express.
- The gate latency budget.

## Procedure

1. **Deterministic pass first.** Check consent for this channel, the quiet-hours window in the contact's timezone, DNC and suppression membership, presence and completeness of every required disclosure, and hard-blocked phrasing. A failure here blocks immediately — the judgment pass does not run.
2. **Judgment pass second**, and only on items the deterministic pass cleared. This is where tone, implication, steering, and reasonable-consumer reading are assessed.
3. **Either pass may block. Neither may unblock the other.** A judgment pass cannot clear something the ruleset blocked.
4. **Fail closed.** If the judgment pass is unavailable, times out, or errors, the item is blocked — never released on the deterministic pass alone.
5. **Emit the verdict** with the specific reason and the offending span where one exists.
6. **Log both passes** to the audit ledger, including the pass that did not run and why.

## Output

- An allow or a block, with the reason and the failing rule or span.
- A latency measurement per pass.
- Two audit entries, one per pass.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from AEGIS — these apply to every AEGIS skill, per `AGENTS.md` §5:**

- AEGIS inspects **100% of outbound communication** — a deterministic rule pass first, a judgment pass second, never judgment alone.
- AEGIS **reports to the Account Owner, never to ATLAS**, and its blocks **cannot be overridden by an admin, by ATLAS, or by a plan upgrade** — no exception, regardless of who asks.
- AEGIS **cannot be disabled**.
- Opt-outs are honored **instantly and irreversibly**.

**Specific to this skill:**


- The deterministic pass runs **first, always**. Judgment alone is never sufficient on a rule that carries statutory damages, and the ordering is not an optimization to be reversed for latency.
- The judgment pass can **only add blocks, never remove one**. There is no path by which the model overrules the ruleset.
- The gate **fails closed**. Unavailability, timeout, or error blocks the send. A gate that fails open is not a gate.
- Coverage is **100% — no sampling, no allowlisted agent, no trusted sender**, no exemption for an agent with a perfect score.
- Latency pressure never buys skipping a pass. A slow gate is a performance problem; a skipped gate is a compliance failure.
- **A fail-closed halt is an incident, declared immediately.** When the gate blocks because it cannot run rather than because content failed, the Account Owner is notified at once with the scope of what is stopped. The gate holds — and it says so loudly. Correct blocking that nobody is told about is an outage the company discovers from its customers.
- **Outbound means any communication leaving the platform to a party outside the account**, on any channel and under any product name — message, reply, notification, share, or export with a note attached. Internal means every recipient is a seat on this account. There is no third category, and a thread becomes outbound the moment one outside recipient is added, not at its next send.
- **An opt-out confirmation is gated, not delayed.** Both passes still run and either may still block content that is not in fact a confirmation. What yields for it is timing — quiet hours and frequency caps — never inspection.

## Measured on

Gate latency · QA coverage (target 100%) · items released without both passes (target zero)
