---
name: pre-signoff-discrepancy-flag
description: Catches mismatches between contracted terms and actual settlement before anyone signs off on a payout, and makes the flag impossible to miss without dismissing it. Fires before every payout authorization.
agent: TALLY
division: Operations
binding: mandate
---

# Pre-Signoff Discrepancy Flag

TALLY cannot stop a payout. What it can do is make it impossible to authorize one without having seen the discrepancy.

## When this fires

- Before every payout authorization, without exception.
- When contracted terms and actual settlement diverge on any file in the payout.
- When a file in the payout carries an open delta or an unresolved flag.
- When a payout is assembled under a plan version that does not match the files' close dates.

## Inputs

- The assembled payout and every file in it.
- Contracted terms per file, and actual settlement.
- Open deltas from reconciliation, and any flags from the close-time calculation.
- The plan versions applicable to each file.

## Procedure

1. **Compare contracted terms against actual settlement on every file in the payout.**
2. **Surface every discrepancy before authorization, never after.**
3. **Present the flag where the authorization happens**, attached to the payout, not filed in a report elsewhere.
4. **Require the flag to be explicitly dismissed with a stated reason**, rather than passively cleared by proceeding.
5. **Record the dismissal and its reason** alongside the authorization.
6. **Never suppress a flag because the payout is time-critical.**
7. **Report the flag to the affected producer as well**, so the discrepancy is not resolved entirely without them.

## Output

- A discrepancy flag per affected file, presented at the point of authorization.
- An explicit dismissal with a stated reason, recorded against the authorization.
- A copy of the flag to the affected producer.
- A preservation hold on the files and documents the discrepancy concerns, held until the flag closes.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from TALLY — these apply to every TALLY skill, per `AGENTS.md` §5:**

- TALLY **calculates, reconciles, and reports** — it **never moves money, never issues a payment, and never files anything**.
- **Every payout requires human authorization.** TALLY's computation is an input to that decision, never a substitute for it.
- TALLY is **not a tax or accounting professional** and never advises as one.

**Specific to this skill:**

- **TALLY cannot block a payout, and it never pretends to.** It is advisory on all money movement — what it guarantees instead is that no payout is authorized without the discrepancy having been presented and explicitly dismissed with a reason. Advisory does not mean quiet.
- **A flag is dismissed explicitly, never cleared by proceeding.** A warning that disappears when someone clicks past it is a warning that will be clicked past every time, and the dismissal record is the only thing that makes the flag real.
- **No flag is suppressed for time pressure.** A payout that has to happen today is exactly the payout where a discrepancy is least likely to get a second look, which is when the flag is worth the most.
- **The flag reaches the affected producer too.** A discrepancy in someone's compensation resolved entirely without them, by dismissal, is how a correctable error becomes a grievance.
- **The flag states the mismatch, not its cause.** "Contracted split is 60/40, settlement reflects 55/45" is TALLY's output; why that happened, and whether it was permitted, is a human determination.
- **Every dismissal is recorded with its reason and its authorizer**, permanently, and the pattern of dismissals is itself reportable.
- **An open discrepancy places a preservation hold on the files it concerns.** A compensation dispute is settled on the settlement statements and the contracted terms behind it; VAULT disposes of documents on schedule and on approval, and a dispute that outlives the retention window loses its own evidence unless something placed the hold.

## Measured on

Discrepancies caught before payout · payouts authorized without the flag presented (target zero) · dismissal rate and its pattern
