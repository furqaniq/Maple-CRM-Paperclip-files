---
name: close-handoff
description: Runs pre-close and post-close sequences, then transfers lifetime ownership of the file to EMBER with the context that makes a future touch defensible. Fires at pre-close and again at post-close.
agent: FORGE
division: Revenue
binding: mandate
---

# Close Handoff

The close is the moment the file becomes a relationship, and the handoff is what decides whether anyone ever has a real reason to call again.

## When this fires

- At the pre-close point on every file.
- At post-close, on funding or completion.
- On a close that falls through, which hands back rather than forward.

## Inputs

- The closed file, its terms as recorded, and its full condition and document history.
- The per-contact brief from ATLAS's `memory-brief-store`.
- The contact's channel consent and permanent exit state.
- EMBER's next-touch requirements — what a future trigger will need to fire on.

## Procedure

1. **Run the pre-close sequence** — outstanding items, party confirmations, and what each party must do before the date.
2. **Run the post-close sequence** on completion, including document delivery and the final status broadcast.
3. **Assemble the handoff record**: the file's position as recorded, its key dates, what the customer said mattered to them, and the anniversaries and eligibility windows a future trigger could fire on.
4. **Transfer lifetime ownership to EMBER's `next-touch-clock`**, and confirm EMBER accepted it rather than assuming.
5. **Carry consent and permanent exit state forward unchanged.** A close is not a new consent.
6. **Hand back to the pipeline rather than forward** where a close falls through, and never hand a fallen-through file to a retention track as though it closed.
7. **Record the handoff**, so a file that belongs to neither agent is impossible.

## Output

- Completed pre-close and post-close sequences.
- A handoff record carrying position, dates, stated priorities, and future trigger conditions.
- A confirmed transfer of lifetime ownership to EMBER.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from FORGE — these apply to every FORGE skill, per `AGENTS.md` §5:**

- FORGE **never alters terms, rates, locks, or fees.** It surfaces, chases, notifies, and escalates. Changes are human acts with human audit trails.
- Document chasing **always checks the file vault first** before re-requesting from the customer.
- Missed deadlines are a **target-zero** metric — the 72/48/24-hour escalation ladder is mandatory, not optional.

**Specific to this skill:**

- **The handoff is confirmed, not assumed.** A file that FORGE has released and EMBER has not accepted belongs to nobody, and nobody is exactly who will notice it for the next four years.
- **Consent and permanent exit state carry forward unchanged.** Closing a file is not a fresh consent to be marketed to, and a contact who exited a channel during the process stays exited after it.
- **The handoff carries the trigger conditions, not just the history.** EMBER's boundary is that it only reaches out when the math genuinely benefits the contact — and it can only do that if the position, the dates, and the terms as recorded came across.
- **FORGE records the terms; it never restates or adjusts them** — see [`terms-interlock`](../terms-interlock/SKILL.md). What goes in the handoff is what the closing documents say.
- **A fallen-through close hands back to the pipeline, never forward to retention.** Putting a file that did not close onto a post-close nurture track produces congratulations to someone who did not get the loan.
- **Post-close messages state what happened, never what it means for the future.** "You could refinance in two years" at close is a forward-looking figure with no disclosures and no basis.
- **Nothing in the handoff is a figure without its label.** A position estimate that loses its estimate label at close becomes the number EMBER's benefit math runs against four years later.

## Measured on

Pull-through · files handed off without a confirmed acceptance (target zero) · consent or exit state altered at close (target zero) · post-close triggers that fired on complete handoff data
