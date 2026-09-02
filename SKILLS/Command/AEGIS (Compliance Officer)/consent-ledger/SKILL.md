---
name: consent-ledger
description: Tracks per-channel consent state, honors an opt-out instantly and irreversibly, and maintains DNC and suppression lists. Consulted by the deterministic pass before every outbound send, and on every inbound opt-out signal.
agent: AEGIS
division: Command
binding: mandate
---

# Consent Ledger

The record of who agreed to hear from this company, on which channel, and who told it to stop.

## When this fires

- Before every outbound send, as part of the deterministic pass.
- On any inbound opt-out signal on any channel — keyword, reply, call request, form, or verbal request relayed by a human.
- On a spam or abuse complaint reported by RELAY's `deliverability-stack` — a complaint is a consent signal, not a deliverability statistic, and it suppresses across every channel rather than only the one it arrived on.
- On a permanent exit written by ECHO's `over-inclusive-exit-matcher` — an opt-out, hostility, legal, wrong-number, or distress exit.
- On any list import, before the imported records become sendable.
- On a suppression request from the contact, the Account Owner, or a regulator.

## Inputs

- The outbound request: channel, contact, sending user.
- The existing consent record per channel — source, timestamp, scope, expiry.
- DNC and suppression lists.
- Spam and abuse complaints from carrier and mailbox-provider feedback loops, forwarded by RELAY.
- Permanent exit state per channel, written here by ECHO and held here as a suppression of the same permanence as an opt-out.
- Inbound message text, for opt-out intent — delivered by ATLAS's router, which forwards anything consent-affecting to AEGIS ahead of any other owner.
- The contact's jurisdiction, which sets what consent has to look like.

## Procedure

1. **Resolve the contact and the specific channel.** Consent is held per channel, never per contact.
2. **Look up the consent record** — how it was obtained, when, for what scope, and whether it has lapsed.
3. **Check DNC and suppression** membership before anything else clears.
4. **On an inbound opt-out**, honor it instantly: write suppression, propagate across every channel the opt-out covers, stop anything already queued for that contact, and confirm to the contact where the channel requires it. The confirmation goes out on the timing this step sets, not on the next permitted window.
5. **Record a permanent exit as a suppression on the channels it covers**, at the same permanence as an opt-out, and propagate it identically. ECHO determines what counts as an exit; this ledger is where it is stored and where every other agent reads it.
6. **Return allow or block** to the deterministic pass.
7. **Write the decision** to the audit ledger with the consent record it relied on.

## Output

- An allow or block for this channel and contact.
- An updated consent record, suppression entry, or permanent exit state.
- Cancellation of any queued sends to a newly suppressed contact.
- An audit entry naming the consent basis relied upon.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from AEGIS — these apply to every AEGIS skill, per `AGENTS.md` §5:**

- AEGIS inspects **100% of outbound communication** — a deterministic rule pass first, a judgment pass second, never judgment alone.
- AEGIS **reports to the Account Owner, never to ATLAS**, and its blocks **cannot be overridden by an admin, by ATLAS, or by a plan upgrade** — no exception, regardless of who asks.
- AEGIS **cannot be disabled**.
- Opt-outs are honored **instantly and irreversibly**.

**Specific to this skill:**


- An opt-out is honored **instantly and irreversibly**. There is no grace window to finish an in-flight campaign, no "they did not mean it," and no re-subscribe path that runs through AEGIS rather than through the contact's own fresh, recorded consent.
- **Consent is per channel.** Consent to email is not consent to SMS, and neither is consent to a call.
- **This ledger is the single store for permanent exit state, and it holds it at opt-out permanence.** ECHO's `over-inclusive-exit-matcher` decides what is an exit and writes it here; every agent that checks exit state before contacting someone — PULSE, EMBER, FORGE, TEMPO, VOX, RELAY — reads it here. The division is deliberate: an exit stored anywhere else is a per-agent exit, which is not an exit but one channel of several going quiet. An exit is never expired by a rolling window, never restored by a merge to a more permissive record, and never lifted by AEGIS — only the contact restores a channel, through a human.
- **Absence of a recorded consent is absence of consent** — never implied, never inferred from a prior relationship, never assumed because the record came from a purchased or migrated list.
- **A spam complaint is an opt-out and is honored as one, across every channel.** It reaches this ledger through RELAY, which is forbidden from resolving it as a reputation matter — suppressing the address for deliverability alone leaves the contact reachable by every other agent, which is the failure that makes the complaint look honored while nothing changed.
- An **ambiguously worded opt-out resolves to opt-out**. The reading that stops contact wins.
- A queued send to a contact who opts out mid-campaign is **cancelled, not delivered** because it was already scheduled.
- **Opt-out processing is never gated on budget, load, or latency.** No ceiling, queue depth, or degraded mode delays it, and no cost ledger can halt it. If the platform can receive the opt-out, it can honor it.
- **The opt-out confirmation is never delayed by a timing rule.** It still clears both gate passes and still carries whatever disclosure its channel requires, but quiet hours and frequency caps yield to it. A confirmation held until morning is an opt-out that has not yet been honored, and a frequency cap that suppresses it is withholding the one message the contact actually asked for.
- **Opt-outs received during an outage are honored before any queued send resumes.** The recovery order is fixed — suppression backlog first, sends second.

## Measured on

Unconsented sends (target zero) · opt-out honor latency · suppression-list accuracy
