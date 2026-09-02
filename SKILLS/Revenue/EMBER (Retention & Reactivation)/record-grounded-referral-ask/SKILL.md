---
name: record-grounded-referral-ask
description: Builds a specific referral request grounded in the record rather than a generic one that converts near zero, and never offers anything of value in exchange. Fires at the configured referral moment on an eligible contact.
agent: EMBER
division: Revenue
binding: mandate
---

# Record-Grounded Referral Ask

"Do you know anyone?" converts at nothing; a specific ask grounded in what the contact told you converts, and costs nothing to make.

## When this fires

- At the configured referral moment on a contact whose experience supports one.
- On a contact volunteering satisfaction outside a review ask.
- Never during an open complaint, escalation, or dispute.

## Inputs

- The contact's record — what they were trying to do, what they said mattered, who else was involved.
- The account's approved referral phrasing set.
- The contact's channel consent, exit state, and window position.
- Prior referral-ask history on this contact.

## Procedure

1. **Ground the ask in something specific from the record** — the situation they were in, the outcome they got, the kind of person that maps to.
2. **Ask for the specific circumstance rather than for names**: the situation a referral would look like, not a request to hand over a contact list.
3. **Offer nothing of value in exchange**, and route any request to attach an incentive to a human under the account's own compensated-referral rules.
4. **Ask through the approved phrasing set**, never improvised.
5. **Limit the frequency** — a referral ask is dormant-pool outreach and sits inside the twenty-one-day window like everything else.
6. **Take a non-answer as an answer**, and do not re-ask on the same basis.
7. **Record any referral that arrives with its source**, and hand it to SCOUT as a lead with its lineage rather than creating it locally.

## Output

- A specific, record-grounded referral request from the approved phrasing set.
- An arriving referral handed to SCOUT with its source lineage attached.
- A human routing where an incentive was requested.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **Nothing of value is offered for a referral, in any form.** A gift card, a discount, a fee credit, a charitable donation, a prize entry, or a reciprocal arrangement is consideration, and consideration for a referral in this industry is a RESPA question rather than a marketing choice. Any request to attach one goes to a human, and TALLY's `obligation-tracker` is where a legitimately compensated arrangement is recorded — never here.
- **The ask is grounded in the record and specific.** A generic ask converts at approximately zero and costs the relationship the same amount as a good one.
- **Ask for the circumstance, not for a list.** Requesting someone's contacts is a different and much larger ask than describing who the company can help.
- **Approved phrasing only.** An improvised referral ask is unreviewed outbound content on the most relationship-sensitive message the company sends.
- **No ask during an open complaint, escalation, or dispute**, and no re-ask on the same basis after a non-answer.
- **A referral arriving is handed to SCOUT with its lineage.** Creating the lead locally loses the source, and the source is the entire point of tracking referrals.
- **A referred person is a new contact with no consent.** The referrer's consent is not theirs, and first contact starts from nothing — see [`figure-disclosure-route`](../figure-disclosure-route/SKILL.md) for anything carrying a figure.

## Measured on

Referrals generated · referral asks carrying consideration (target zero) · generic asks sent (target zero) · referred leads reaching SCOUT with source lineage intact
