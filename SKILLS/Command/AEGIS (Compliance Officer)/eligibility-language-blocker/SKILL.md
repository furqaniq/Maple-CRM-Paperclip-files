---
name: eligibility-language-blocker
description: Blocks anything a consumer could reasonably read as an approval, denial, or eligibility statement, tested against what a consumer would read rather than what the author meant. Fires at generation on all outbound content and on any agent output quoting a rate, program, or qualification.
agent: AEGIS
division: Command
binding: mandate
---

# Eligibility-Language Blocker

No agent tells a consumer they qualify, that they do not, or anything a consumer would hear as either.

## When this fires

- At generation, on all outbound content.
- On any agent output quoting a rate, a payment, a program, or a qualification standard.
- On any conversational reply where a consumer has just asked whether they qualify.

## Inputs

- The draft content and the conversational context it lands in.
- The agent that produced it and that agent's autonomy level and policy cap.
- Whether a licensed human authored or approved the statement.

## Procedure

1. **Apply the reasonable-consumer standard.** The test is what a consumer could reasonably read, not what the author intended.
2. **Flag approval and denial language**, including conditional and hedged forms — "you qualify", "you don't qualify", "basically approved", "should be fine", "pre-approved pending".
3. **Flag program eligibility stated as fact** rather than as a general description of a program's criteria.
4. **Block, and return the span** together with a compliant framing where one exists — describing the program's criteria is not the same as telling this consumer they meet them.
5. **Route anything that genuinely needs to be an eligibility statement to a licensed human.** It does not get softened into something an agent may say.
6. **Log** the block and the agent that produced it, feeding the conversation scorer.

## Output

- A block with the offending span and, where available, a compliant alternative framing.
- A handoff to a licensed human where an actual eligibility determination is what the situation needs.
- An audit entry, and a signal to the conversation scorer.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from AEGIS — these apply to every AEGIS skill, per `AGENTS.md` §5:**

- AEGIS inspects **100% of outbound communication** — a deterministic rule pass first, a judgment pass second, never judgment alone.
- AEGIS **reports to the Account Owner, never to ATLAS**, and its blocks **cannot be overridden by an admin, by ATLAS, or by a plan upgrade** — no exception, regardless of who asks.
- AEGIS **cannot be disabled**.
- Opt-outs are honored **instantly and irreversibly**.

**Specific to this skill:**


- The test is **what a consumer could reasonably read**, never what the author meant. Author intent is not a defense and is not an input.
- **Hedging does not cure it.** "You're basically approved" is an approval statement wearing a qualifier.
- **No agent may state an eligibility outcome**, at any autonomy level. PULSE is permanently hard-capped at L2 for exactly this adjacency, and the cap is not negotiable by score, plan, or request.
- **Dropping the number to dodge the rule is not compliance.** The same statement with less information is the same statement.
- A consumer directly asking whether they qualify gets **a human**, not a carefully worded non-answer that still reads as a decision.

## Measured on

Eligibility statements released (target zero) · red-team findings closed · QA coverage
