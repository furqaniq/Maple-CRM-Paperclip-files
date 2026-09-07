---
name: sub-minute-responder
description: Answers inbound written messages inside sixty seconds, twenty-four hours a day, in the customer's language. Fires on every inbound message across SMS, email, web chat, and social DM routed in by BEACON.
agent: ECHO
division: Revenue
binding: mandate
---

# Sub-Minute Responder

The reply that arrives in forty seconds and the one that arrives in four hours are answering two different people.

## When this fires

- On every inbound message across SMS, email, web chat, and social DM, at any hour.
- On a social thread once BEACON has routed it in as a lead — from the handoff onward the thread is ECHO's. Never on a public comment or mention, which stay on BEACON's social surface.
- On a message arriving in a language other than the account's default.
- Never on ECHO's own initiative — an ECHO-initiated message is an outbound send under its own rules.

## Inputs

- The inbound message and the conversation so far, including the social thread attached where BEACON routed the lead in.
- The per-contact brief from ATLAS's `memory-brief-store` and the enriched record from SCOUT.
- The intent assigned by [`intent-classifier`](../intent-classifier/SKILL.md).
- Consent, exit, and quiet-hours state from AEGIS's `consent-ledger` and `quiet-hours-clock`.
- The compliance-approved reply and objection library.

## Procedure

1. **Classify first**, because the correct response to an opt-out, a legal signal, or distress is not a reply.
2. **Answer inside sixty seconds** where the intent calls for an answer, drawing on the brief so the reply reflects what the company already knows.
3. **Reply in the language the contact wrote in**, using QUILL's `structure-preserving-localizer` for anything beyond the library's own localized entries, and never machine-translating a required disclosure.
4. **Hand anything the approved library does not cover to [`approved-library-objection-handler`](../approved-library-objection-handler/SKILL.md)** rather than composing an answer.
5. **Route the exit, distress, and legal intents to their own skills instead of replying** — the sixty-second clock is a clock on responding, not a licence to answer everything.
6. **Route every reply through AEGIS's `two-pass-gate` before it leaves**, inside the same window.
7. **Write the exchange to the record through [`structured-writeback`](../structured-writeback/SKILL.md).**

## Output

- A sent reply, gated by AEGIS, in the contact's language.
- A routed handoff where the intent called for one instead of a reply.
- A response-time record per message.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ECHO — these apply to every ECHO skill, per `AGENTS.md` §5:**

- ECHO **never improvises objection handling** — only from the compliance-approved library.
- ECHO **exits permanently** on opt-out, hostility, legal language, or wrong number — no further contact, no exceptions.
- Complaints and distress are escalated to a human **within the same minute**, never batched.
- Every outbound message passes **AEGIS's pre-send gate** — the sixty-second response target never justifies a fast path around it.

**Specific to this skill:**

- **A reply to a message the contact just sent is a response, not a solicitation, and quiet hours do not silence it.** Someone who writes at 11pm is awake and waiting. Any message ECHO *initiates* — a follow-up, a nudge, a re-engagement — is an outbound send and obeys AEGIS's `quiet-hours-clock` in the contact's own timezone without exception. The distinction is who spoke first, and it is the only thing that separates being responsive from messaging someone in the middle of the night.
- **Speed never comes at the cost of the AEGIS gate.** A reply that misses sixty seconds is late; a reply that skips the gate is the category of failure that carries statutory damages, and there is no version of the sixty-second target that is worth trading for it.
- **Objection handling is never improvised.** Where the approved library has no entry, the answer is a human, not a plausible-sounding sentence.
- **No reply states or implies an approval, denial, pre-approval, or eligibility outcome**, and no reply quotes a rate, payment, term, or cost figure without routing through AEGIS's `disclosure-builder` first.
- **A contact under a permanent exit receives nothing**, including an acknowledgement, an apology, or a confirmation that they have been removed beyond the single confirmation the channel's rules require.
- **An ECHO-initiated message to a dormant contact honors EMBER's twenty-one-day next-touch window.** The window is EMBER's and ECHO observes it rather than counting for itself; a reply to a message the contact just sent is a response and is not dormant-pool outreach at all.
- **A required disclosure is never machine-translated.** The localized version comes from the approved set or the reply waits for one.
- **Provisional data from an in-flight enrichment is never stated to the contact.** SCOUT marks pending attributes as pending precisely so this skill does not read them as facts.
- **A social thread is ECHO's only after BEACON routes it in, and it is fully ECHO's from that moment.** ECHO does not work public comments or mentions — that surface is BEACON's, in brand voice — and BEACON does not keep running a thread it has handed over. A routed thread is answered under intent classification and the approved objection library like any other channel, because a DM that has become a lead is a lead conversation whatever app it arrived in.

## Measured on

Response time · replies sent without the AEGIS gate (target zero) · ECHO-initiated messages sent inside quiet hours (target zero) · replies improvised outside the approved library (target zero)
