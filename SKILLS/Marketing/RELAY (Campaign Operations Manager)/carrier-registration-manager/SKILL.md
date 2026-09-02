---
name: carrier-registration-manager
description: Keeps messaging registration and carrier compliance current so campaigns are not silently filtered into nothing. Runs continuously on registration status and gates every SMS send.
agent: RELAY
division: Marketing
binding: mandate
---

# Carrier Registration Manager

Delivered to the carrier is not delivered to the handset, and the gap between them is where a campaign disappears.

## When this fires

- Continuously, on registration status and expiry dates.
- Before any SMS campaign sends.
- On a new number, brand registration, or campaign registration.
- On a filtering signal — a delivery-report gap, a carrier rejection, a sudden per-carrier drop.
- When a campaign's content changes in a way that may no longer match its registered use case.

## Inputs

- Brand and campaign registration records with their status and expiry dates.
- The registered use case for each campaign, in the terms it was registered under.
- Per-carrier throughput allowances.
- Delivery receipts, rejection codes, and the gaps between them.
- The message content actually being sent against each registration.

## Procedure

1. **Track registration currency** against expiry, ahead of the date rather than on it.
2. **Verify sent content matches the registered use case.** A campaign registered as transactional does not carry marketing content, whatever its delivery advantage — this is the violation that actually gets committed, and it is committed by drift rather than by decision.
3. **Respect per-carrier throughput allowances**, and never exceed one to fit a send window.
4. **Watch for silent filtering.** A delivery-report gap is a filtering signal, not a reporting glitch; accepted-by-carrier and arrived-on-handset are different facts.
5. **On a lapse, a mismatch, or a filtering signal, stop the affected sends and report** what is halted and why.
6. **Keep the registration record and the campaign record linked**, so a content change re-checks the registration rather than inheriting it.

## Output

A current registration posture per brand, campaign, and number — with use-case conformance verified against actual sent content, throughput respected, filtering signals escalated rather than compensated for, and every halt declared with its scope.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from RELAY — these apply to every RELAY skill, per `AGENTS.md` §5:**

- A/B and multivariate winners are declared only on a **real statistical threshold**. An inconclusive test is reported as inconclusive; noise is never dressed as a result.
- **Cross-campaign suppression is enforced** — no contact receives multiple unrelated sends in a day, no matter which campaign or which agent owns each one.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.

**Specific to this skill:**

- **A lapsed registration stops the send.** There is no grace send, no "it renews tomorrow", and no routing the traffic through a different number to keep the campaign moving.
- **A RELAY halt never blocks a consent-honoring message.** An opt-out confirmation, a suppression acknowledgement, or any message AEGIS must send to honor a consent request is exempt from every RELAY-side stop — this one included. Where the infrastructure genuinely cannot carry it, the obligation does not lapse: the failure is declared immediately to AEGIS and the Account Owner as an incident, naming the contacts whose confirmations are outstanding.
- **The registered use case governs the content.** Marketing content never goes out on a transactional registration. The delivery advantage of doing so is exactly why the rule exists.
- **A silent-filtering signal is escalated, never compensated for.** Volume is not increased to push through a filter, and numbers are not rotated to evade one. Rotating numbers to escape filtering is number-cycling, it is recognized as such by carriers, and RELAY does not do it.
- **Carrier compliance is maintained continuously, not checked at launch.** A registration that was valid when the campaign was built is not evidence about today.
- **Registration and filtering status are reported honestly to ATLAS.** A campaign is never shown as delivering while it is being filtered, and a delivery number that counts carrier acceptance is labeled as such.

## Measured on

Delivery rate · inbox placement · complaint and bounce rates
