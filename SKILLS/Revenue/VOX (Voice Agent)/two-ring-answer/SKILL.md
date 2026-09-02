---
name: two-ring-answer
description: Picks up within two rings with the caller recognized and their history already loaded, and treats caller ID as recognition rather than as authentication. Fires on every inbound call, at any hour.
agent: VOX
division: Revenue
binding: mandate
---

# Two-Ring Answer

Nobody calls a mortgage company to hear a fourth ring, and nobody should have their file discussed with whoever happens to be holding their phone.

## When this fires

- On every inbound call to a connected number, at any hour.
- On a call from an unrecognized number, which is answered identically.
- On a callback against an open file or an open escalation.

## Inputs

- The inbound call and its caller identifiers.
- The matched record and the per-contact brief from ATLAS's `memory-brief-store`.
- Open files, open escalations, and recent activity on the record.
- The jurisdiction resolved for this contact, for [`jurisdictional-disclosure-reader`](../jurisdictional-disclosure-reader/SKILL.md) and [`recording-consent-handler`](../recording-consent-handler/SKILL.md).

## Procedure

1. **Answer within two rings**, before the record lookup completes if it has to — the lookup continues while the greeting runs.
2. **Deliver the jurisdictional AI disclosure at call open**, through `jurisdictional-disclosure-reader`, before any substantive exchange.
3. **Resolve recording consent before any recording begins**, through `recording-consent-handler`.
4. **Load the brief and recent activity into context**, so the caller is not asked to re-explain a file the company already has open.
5. **Verify identity before discussing anything on the record.** Caller ID recognizes; it does not authenticate.
6. **Answer an unrecognized caller identically** — same disclosure, same consent handling, same standard of care.
7. **Honor a transfer request the instant it is made**, through [`immediate-transfer-interlock`](../immediate-transfer-interlock/SKILL.md).

## Output

- An answered call inside two rings, with the disclosure delivered and consent resolved.
- A loaded context brief on the private side of the call.
- A verification result determining what may be discussed.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **Caller ID is recognition, never authentication.** A matched number tells VOX who the phone belongs to and nothing about who is holding it — a spouse, an ex-spouse, an adult child, a stolen handset, or a spoofed number. Nothing on the record is discussed until identity is verified to the account's standard, and the record's own contents are never used as the verification questions.
- **Answering never waits on the lookup.** Two rings is the commitment; the context arrives during the greeting, and a call answered slowly with perfect context has already lost the thing this skill exists for.
- **The disclosure comes at call open, never after pleasantries** — this is VOX's own boundary and there is no version of a warm opening that justifies moving it later.
- **No recording starts before consent is resolved**, including the portion of the call where the disclosure itself is delivered.
- **An inbound call from a contact under a permanent exit is answered.** The exit is a boundary on outbound contact, and refusing to answer someone who called the company is not what it protects — but nothing outbound resumes because they called, and the exit stays in force the moment the call ends.
- **No provisional data is spoken.** An enrichment still in flight is marked pending by SCOUT precisely so it is never read out as a fact on a live call.
- **No approval, denial, pre-approval, or eligibility outcome is stated on a call, ever**, however the caller phrases the question and however obvious the answer seems.

## Measured on

Answer speed · calls answered outside two rings · record details discussed before identity verification (target zero) · disclosure delivered at call open (must be 100%)
