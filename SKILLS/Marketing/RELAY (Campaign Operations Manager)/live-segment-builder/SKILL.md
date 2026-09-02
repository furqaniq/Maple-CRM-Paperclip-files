---
name: live-segment-builder
description: Builds audiences from CRM data with segment logic that stays live and resolves at send time rather than freezing at export. Fires when a campaign needs an audience, and again at every send against that audience.
agent: RELAY
division: Marketing
binding: mandate
---

# Live Segment Builder

An exported list is a snapshot of who had not opted out yet.

## When this fires

- A campaign needs an audience defined.
- A scheduled send references a segment — membership resolves again, at send time, not at build time.
- Segment criteria change, or the underlying CRM data model changes.
- A consent, suppression, or deliverability event changes who is eligible.

## Inputs

- CRM contact records and the segment logic expressed over them.
- **Consent state from AEGIS's consent ledger**, read live — the ledger is the authority, not a CRM field copy of it.
- Frequency suppression state, as a separate input from consent.
- Deliverability exclusions: hard-bounced addresses, complained addresses, unreachable numbers.
- The account's licensed-state footprint.
- The criteria themselves, as text, for screening.

## Procedure

1. **Express the audience as live logic** over CRM state, never as a materialized list.
2. **Resolve membership at send time.** A contact who opted out between build and send is not in the audience, and no cached membership overrides that.
3. **Apply consent state as an exclusion, read from AEGIS's consent ledger** at resolution.
4. **Apply frequency suppression separately**, through [`cross-campaign-suppression`](../cross-campaign-suppression/SKILL.md), which is a different mechanism with a different lifetime.
5. **Apply deliverability exclusions** — hard bounces and complained addresses.
6. **Screen the criteria themselves** for protected characteristics and their proxies: zip code and neighborhood used as demographic selectors, surname pattern, language, national-origin markers, age-coded fields. Screening the output list is not enough; the criteria are the discriminatory act.
7. **Emit with the count, the criteria as written, every exclusion applied and its reason, and the resolution timestamp.**

## Output

A resolved audience with its count, the criteria in the words they were written, each exclusion category and how many it removed, and the timestamp of resolution — never a stored list that a later send reuses.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from RELAY — these apply to every RELAY skill, per `AGENTS.md` §5:**

- A/B and multivariate winners are declared only on a **real statistical threshold**. An inconclusive test is reported as inconclusive; noise is never dressed as a result.
- **Cross-campaign suppression is enforced** — no contact receives multiple unrelated sends in a day, no matter which campaign or which agent owns each one.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.

**Specific to this skill:**

- **A segment resolves at send time.** An exported list is a snapshot and is never the thing sent to, however recently it was taken.
- **Consent state is read live from AEGIS's consent ledger, never from a cached CRM field.** A stale copy of consent is the specific mechanism by which an honored opt-out gets violated a week later.
- **Consent exclusion and frequency suppression are different mechanisms and never share a store.** A frequency window expires by design; an opt-out does not expire at all. Merging them means a window rolling over can return a contact who opted out — which is the worst failure this agent is capable of.
- **Segment criteria are screened for protected characteristics and their proxies** to the same standard QUILL applies to copy. A targeting decision is as much a fair-lending act as a sentence is.
- **A segment for a campaign carrying imagery is delivered to CANVAS's demographic screen before the send.** Neutral imagery and a demographically shaped audience are one violation split across two agents, and only RELAY holds the half that makes it visible. Withholding the segment does not make the pairing compliant, it makes it unscreened.
- **A segment RELAY cannot resolve does not fall back to a broader one.** It stops, reports what could not be resolved, and the send waits. Sending to a superset because the subset failed is how a suppressed contact gets reached.
- **A segment never extends beyond the licensed-state footprint** for a solicitation the account is not licensed to make.

## Measured on

Cost per engaged contact · complaint and bounce rates · delivery rate
