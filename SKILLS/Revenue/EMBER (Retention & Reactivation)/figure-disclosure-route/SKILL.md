---
name: figure-disclosure-route
description: Routes any message containing a rate, payment, term, or cost figure through AEGIS's disclosure builder automatically, and never drops the number to dodge the rule. Fires on every EMBER message before it leaves.
agent: EMBER
division: Revenue
binding: interlock
---

# Figure Disclosure Route

This is the handbook's hard boundary for EMBER: a figure goes out with its disclosures, or the message does not go out — and removing the figure to avoid the rule is not an option either.

## When this fires

- On every EMBER message, before it leaves, screened for a rate, payment, term, or cost figure.
- On any figure arriving in an attachment, an image, a linked report, or a co-branded asset.
- On any request to send a figure without disclosures, or to remove a figure to avoid them.

## Inputs

- The composed message and everything attached, linked, or embedded in it.
- The figures present, their basis, and their estimate labels.
- The sending user's profile and licensing, for AEGIS's `disclosure-builder`.
- The requester's identity and role, on any request to bypass.

## Procedure

1. **Detect figures across every surface of the message** — body, subject line, attachment, image, linked report, and co-branded asset.
2. **Route every detected figure through AEGIS's `disclosure-builder`** and attach what it returns, at the point in the message where the figure appears.
3. **Refuse to send where the disclosures cannot be assembled**, rather than sending the figure without them.
4. **Refuse to remove the figure to avoid the rule.** This step has no branch. The message either goes with disclosures or does not go.
5. **Refuse the indirect paths identically** — a figure in a screenshot, a figure in a linked PDF, a figure a partner's co-branded template supplies, a figure spelled out in words.
6. **Verify the estimate labels survived**, so a benefit calculation does not ship as a firm number.
7. **Record the routing, the disclosures attached, and any refusal**, to AEGIS's `audit-ledger`.

## Output

- A sent message with disclosures attached at each figure.
- A refusal where the disclosures could not be assembled, or where a bypass was requested.
- An audit record of the figures, the disclosures, and the outcome.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **This is the handbook's stated hard boundary for EMBER and is not configurable.** Any message with a rate, payment, term, or cost figure routes through AEGIS's disclosure builder automatically.
- **EMBER never drops the number to dodge the rule.** This is the second half of the boundary and the half that gets forgotten. Rewriting "you could save $340 a month" into "you could save meaningfully" removes the disclosure obligation and the contact's ability to evaluate the claim at the same time — it makes the message less regulated and less honest in one edit. The figure stays and the disclosures come with it, or the message does not go.
- **Detection covers every surface, not the body text.** A figure in an image, a screenshot, a linked report, a PDF, a co-branded partner template, or spelled out in words is a figure. A detector that reads only the body is a detector that will be routed around within a month.
- **A figure that cannot carry its disclosures is not sent.** Not with a link to them, not with them in a footer of a linked page, not with a promise to provide them on request.
- **Estimate labels survive the routing.** A benefit figure derived from a property-derived position estimate is an estimate, and shipping it as a firm number is a different violation arriving through the same door.
- **No seniority changes the answer.** Not the Account Owner, not an admin, not a producer who says the contact already knows the number, not ATLAS, not a campaign deadline.
- **A refusal explains what is missing.** A boundary that blocks without naming the missing disclosure gets worked around by someone sending the message from their own email instead.

## Measured on

Figures shipped without disclosures (target zero, and any non-zero value is an incident) · figures removed to avoid the disclosure rule (target zero) · figures detected in non-body surfaces · estimate labels lost in routing (target zero)
