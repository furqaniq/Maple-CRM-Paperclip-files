---
name: partner-value-drop
description: Nurtures partners and sphere with market data, co-marketing, and their own business promoted — never "just checking in." Fires on the partner cadence and on any market event genuinely relevant to a partner.
agent: EMBER
division: Revenue
binding: mandate
---

# Partner Value Drop

A partner touch is only worth making if the partner gets something out of it, and "just checking in" is the touch that gets nothing.

## When this fires

- On the partner cadence, where a genuine value item exists to send.
- On a market event materially relevant to a specific partner's business.
- On a partner's own milestone the company can legitimately promote.

## Inputs

- The partner, their business, their market, and what they have engaged with before.
- Market data from VANTAGE's `client-ready-report-builder` and `geo-granular-market-feed`.
- Co-marketing assets from CANVAS and QUILL, cleared through AEGIS.
- The partner's channel consent, exit state, and window position.

## Procedure

1. **Require a genuine value item before composing anything.** No item, no touch.
2. **Match the item to that partner's actual market and business**, not to a general list.
3. **Send market data with its source and as-of date intact**, exactly as VANTAGE published it.
4. **Route co-marketing assets through CANVAS's `pre-publication-brand-review` and AEGIS**, like any other outbound asset.
5. **Promote the partner's own business where the account has agreed to** and where doing so carries no compensation implication.
6. **Apply the twenty-one-day window and consent state**, since a partner is a contact.
7. **Skip the cadence rather than send filler**, and record the skip so an empty cadence is visible as a content problem.

## Output

- A partner touch carrying a genuine, market-matched value item.
- A recorded skip where no genuine item existed.
- Co-marketing assets cleared through the brand and compliance gates.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **No touch without a genuine value item — this is EMBER's own boundary.** "Just checking in" is explicitly what this skill exists to replace, and it is what an empty cadence reverts to under pressure to keep sending.
- **A skipped cadence is recorded as a content gap, not hidden.** A cadence quietly running on filler looks identical in the metrics to one running on value.
- **Nothing of value is exchanged for referrals in a partner relationship.** Co-marketing, promotion, and shared market data have a compensated-referral shape in this industry, and any arrangement with consideration in it goes to a human and is recorded through TALLY's `obligation-tracker`. EMBER never originates one.
- **Market data keeps VANTAGE's source and as-of date.** A partner forwarding a stale figure to their own client with the company's name on it is a problem that arrives back much later.
- **Partners are contacts.** Consent, exit state, quiet hours, and the twenty-one-day window all apply, and a partner who asked to stop hearing from the company has asked exactly that.
- **Co-marketing goes through the brand and compliance gates.** An asset carrying two companies' names is not less regulated for having a second one on it.
- **Anything with a rate, payment, term, or cost figure routes through [`figure-disclosure-route`](../figure-disclosure-route/SKILL.md)** — a market data drop to a partner is where a figure most easily escapes the disclosure path.

## Measured on

Partner engagement rate · touches sent without a value item (target zero) · skipped cadences recorded as content gaps · co-marketing assets shipped without the brand and compliance gates (target zero)
