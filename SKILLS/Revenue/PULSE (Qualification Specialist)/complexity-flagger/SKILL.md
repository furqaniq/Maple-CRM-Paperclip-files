---
name: complexity-flagger
description: Surfaces files requiring immediate human handling before anyone wastes time on them, and before an agent says something it should not. Fires the moment a complexity signal appears, never on a scheduled pass.
agent: PULSE
division: Revenue
binding: mandate
---

# Complexity Flagger

The expensive complexity is the kind an automated conversation will walk straight into, confidently, ten messages before anyone notices.

## When this fires

- The moment a complexity signal appears in discovery, in a conversation, or in the record.
- On any file characteristic the account has configured as human-only.
- When an agent's confidence drops on anything touching money, deadlines, or legal exposure.

## Inputs

- Discovery answers and live conversation content.
- The enriched record and any unusual property, ownership, or file characteristics.
- The account's configured human-only conditions.
- ATLAS's `confidence-tripwire` state for the contact.

## Procedure

1. **Detect the signal on appearance**, not on a later pass — the value of this skill is entirely in when it fires.
2. **Name the specific complexity** and what makes it one, rather than raising a generic flag.
3. **Stop the automated path on the file immediately**, including any queued sequence, before the handoff completes.
4. **Route to a named human**, not to a queue, with the evidence and the conversation so far attached.
5. **Hand distress, hardship, and legal signals to ECHO's `distress-escalator` in the same minute**, rather than treating them as complexity to be triaged.
6. **Keep the file human-held until a human releases it**, and never auto-resume on a timer.
7. **Record the flag, the routing, and the resolution**, so the account learns which signals actually needed a human.

## Output

- A named complexity flag with the specific signal and the evidence.
- An immediate stop on the automated path for that file.
- A routed handoff to a named human, with context attached.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from PULSE — these apply to every PULSE skill, per `AGENTS.md` §5:**

- PULSE **never states or implies an approval, denial, pre-approval, or eligibility outcome** — to a customer, in any channel, at any confidence level.
- A disqualification is an **internal routing state only** — never communicated to a customer as a decision.
- Autonomy is **permanently capped at L2** — this cap cannot be raised by AEGIS promotion or any other mechanism.

**Specific to this skill:**

- **The flag stops the automated path before the handoff completes, not after.** A file that keeps conversing while it waits for a human is exactly the window in which an agent says the thing that creates the problem.
- **A flagged file does not auto-resume.** Only a human releases it. A timer-based resume is the same as no flag, delayed.
- **Distress, hardship, and legal language are escalated in the same minute as a human matter, not triaged as complexity.** They are ECHO's escalation under ECHO's boundary, and routing them through a complexity queue is how a same-minute rule becomes a same-afternoon rule.
- **A flag names the specific signal.** "Complex file" tells the human nothing and gets ignored on the fourth one.
- **Complexity is a property of the file, never of the person.** Language, national origin, immigration status, age, and household composition are not complexity signals, and treating them as ones routes people to a slower path on a protected characteristic.
- **Over-flagging is a measured cost, not a free safety.** A flagger that routes a third of the pipeline to humans has removed the agent from the process and will be switched off, which leaves the genuinely complex files unflagged.
- **PULSE never resolves the complexity it flags.** It surfaces and hands over; the handling is the human's, and any figure or explanation the human needs comes with its estimate labels intact.

## Measured on

Complexity caught early · automated messages sent on a file after its signal appeared (target zero) · flags carrying a named specific signal · flag precision on human review
