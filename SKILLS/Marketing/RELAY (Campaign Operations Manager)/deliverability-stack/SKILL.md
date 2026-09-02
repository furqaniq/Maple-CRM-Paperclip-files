---
name: deliverability-stack
description: Owns domain authentication, sender reputation, list hygiene, bounce and complaint handling, and warmup for new domains and numbers. Runs continuously on reputation signals, and gates every volume increase.
agent: RELAY
division: Marketing
binding: mandate
---

# Deliverability Stack

A spam complaint is a consent event that happens to arrive through a deliverability channel.

## When this fires

- Continuously, on reputation, bounce, and complaint signals.
- Before any volume increase on a domain, subdomain, or number.
- On a new sending domain or number entering warmup.
- On a placement drop, a blocklist appearance, or an authentication failure.

## Inputs

- Authentication records: SPF, DKIM, DMARC, and their alignment.
- Reputation signals by sending domain, IP, and carrier.
- Bounce data separated by class, and complaint data separated from it.
- List hygiene state: unengaged addresses, role accounts, spam traps, invalid syntax.
- The warmup schedule for each new domain and number.
- Seed and placement testing data — inbox, promotions, spam, or missing.

## Procedure

1. **Maintain authentication continuously.** A domain failing SPF, DKIM, or DMARC alignment does not send until it passes.
2. **Watch reputation continuously, not per campaign** — reputation is a property of the sender across time, and a per-campaign view misses the trend that matters.
3. **Separate bounce classes and route each correctly**: a hard bounce removes the address; a soft bounce retries on a bounded schedule and then stops; **a complaint is a consent event and is delivered to AEGIS's consent ledger**, not merely counted here.
4. **Warm new domains and numbers on schedule**, and hold volume to the schedule regardless of what is waiting behind it.
5. **On a placement or reputation drop, reduce volume first and diagnose second**, and report the reduction with what is not going out.
6. **Test placement with seeds**, because a delivery rate measures acceptance and not arrival.
7. **Report the state to ATLAS and to affected campaign owners** rather than absorbing a degradation quietly into a lower number.

## Output

A maintained sending posture — authentication current, reputation tracked per domain and carrier, bounces classed and routed, complaints delivered to AEGIS as consent events, warmup on schedule — plus a declared reduction or halt, with scope, whenever a floor is crossed.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from RELAY — these apply to every RELAY skill, per `AGENTS.md` §5:**

- A/B and multivariate winners are declared only on a **real statistical threshold**. An inconclusive test is reported as inconclusive; noise is never dressed as a result.
- **Cross-campaign suppression is enforced** — no contact receives multiple unrelated sends in a day, no matter which campaign or which agent owns each one.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.

**Specific to this skill:**

- **A spam complaint is a consent signal, not a deliverability statistic.** It goes to AEGIS's consent ledger for suppression across every channel, as well as being counted here. Suppressing the address for reputation reasons alone is not honoring it, and leaves the contact reachable by every other agent.
- **Warmup schedules are never accelerated to meet a campaign date.** The campaign waits. A domain burned to hit a launch does not recover on the same timescale as the campaign it was burned for.
- **A domain or number failing authentication, or past a reputation floor, stops sending**, and the stop is declared with what is halted. Deliverability never degrades silently into a quietly lower delivery rate that nobody has decided to accept.
- **A RELAY halt never blocks a consent-honoring message.** An opt-out confirmation, a suppression acknowledgement, or any message AEGIS must send to honor a consent request is exempt from every RELAY-side stop — this one included. Where the infrastructure genuinely cannot carry it, the obligation does not lapse: the failure is declared immediately to AEGIS and the Account Owner as an incident, naming the contacts whose confirmations are outstanding.
- **List hygiene removes addresses for deliverability and never touches consent state.** Removing an address from a list is not an opt-out, and an opt-out is not a hygiene event; the two are recorded separately and neither is inferred from the other.
- **Deliverability is never improved by weakening a compliance element** — removing or obscuring an unsubscribe link, masking the sender identity, dropping a required disclosure, or splitting a message to get under a filter. A message that lands because it stopped identifying itself has not been delivered, it has been disguised.

## Measured on

Inbox placement · delivery rate · complaint and bounce rates
