---
name: peak-emotion-review-ask
description: Requests a review at peak emotion, once, without incentives, and without routing unhappy customers away from public platforms. Fires at the configured peak-emotion moment and never again.
agent: EMBER
division: Revenue
binding: mandate
---

# Peak-Emotion Review Ask

Ask once, at the right moment, of everyone — the version that filters by expected sentiment is review gating, and it is the thing regulators actually act on.

## When this fires

- At the configured peak-emotion moment — typically at or shortly after close.
- Never a second time on the same contact, whatever the outcome of the first.
- Never while a complaint, escalation, or dispute is open.

## Inputs

- The contact, the completed transaction, and the peak-emotion moment definition.
- Prior review-ask history on this contact.
- Open complaint, escalation, and dispute state.
- The public platforms the account asks on.

## Procedure

1. **Confirm no review ask has been made to this contact before.** This is the first check and it is dispositive.
2. **Confirm no complaint, escalation, or dispute is open**, and defer indefinitely rather than asking during one.
3. **Ask once, at the peak-emotion moment**, plainly and without an incentive of any kind.
4. **Offer the same public platforms to everyone**, with no pre-screening question and no branch on expected sentiment.
5. **Take no answer as an answer.** No reminder, no second ask, no rephrasing through a different channel.
6. **Route a complaint that arrives in response to the ask to ECHO's `distress-escalator`**, and never toward a private feedback channel instead of the public one.
7. **Record the ask and its outcome**, so a second one is impossible.

## Output

- A single review request at the peak-emotion moment, with no incentive attached.
- A deferral where a complaint or dispute was open.
- An escalation where the ask surfaced a complaint.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **Once, ever — this is EMBER's own boundary.** No reminder, no follow-up, no second ask through another channel, no re-ask on a later transaction with the same person. A review request that is repeated is pressure, and pressure on a review is what makes the review worthless and the practice indefensible.
- **No incentives, of any kind, in any form.** Not a discount, not a gift, not an entry into anything, not a donation, not a contribution to a charity. Every one of these is consideration for a review.
- **No pre-screening, and no routing by expected sentiment.** Asking "how did we do?" first and sending only the happy answers to a public platform is review gating. It is the specific practice enforcement actions are built on, and it is what this rule exists to forbid — the ask goes to everyone, and it names the same public platforms for everyone.
- **An unhappy customer is never routed away from a public platform.** Offering them a private feedback form *instead* is the gate wearing a service face. A private channel may be offered in addition, never as a substitute, and never in place of naming the public one.
- **No ask during an open complaint, escalation, or dispute.** Asking someone mid-complaint to review the company publicly is either oblivious or an invitation, and both are bad.
- **A complaint surfaced by the ask escalates to a human on ECHO's same-minute clock.** The ask found something real and the response to it is a person, not a workflow.
- **The ask never suggests what to say.** Supplying the wording, the rating, or the themes is authoring the review.

## Measured on

Reviews generated · contacts asked more than once (target zero, and any non-zero value is an incident) · asks carrying an incentive (target zero) · sentiment-based routing away from public platforms (target zero)
