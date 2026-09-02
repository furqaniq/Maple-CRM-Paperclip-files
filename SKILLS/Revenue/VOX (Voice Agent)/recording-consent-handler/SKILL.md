---
name: recording-consent-handler
description: Applies recording-consent rules by the contact's jurisdiction before any recording begins. Fires before every recording, on every call, inbound and outbound.
agent: VOX
division: Revenue
binding: interlock
---

# Recording Consent Handler

This is the handbook's hard boundary for VOX: recording consent is handled per jurisdiction, and nothing is recorded until it is resolved.

## When this fires

- Before any recording begins, on every call.
- On a mid-call jurisdiction change, or a third party joining the call.
- On any request to record, retain, or re-process a call whose consent did not permit it.

## Inputs

- The resolved jurisdiction and the basis on which it was resolved.
- The consent rule for that jurisdiction — all-party, one-party, or a specific notice requirement.
- The contact's response to the consent request, where one is required.
- The parties on the line, including any who joined after the open.

## Procedure

1. **Resolve the jurisdiction before the call connects**, and apply the most protective applicable rule where two could apply.
2. **Request consent in the required form at call open** where the jurisdiction requires it, before any recording begins.
3. **Start no recording until the decision is resolved.** This step has no branch — the pre-consent portion of the call is not recorded and is not buffered for later retention.
4. **Proceed with the call unrecorded where consent is refused**, rather than ending it or pressing for consent.
5. **Re-resolve when a third party joins or the jurisdiction changes mid-call**, and stop recording where the new state does not permit it.
6. **Refuse every request to record, retain, or re-process outside what consent permitted**, from any source.
7. **Write the consent state, its basis, and its timestamp to AEGIS's `audit-ledger` and `consent-ledger`.**

## Output

- A resolved recording-consent state, with its jurisdiction basis and timestamp, before any recording.
- An unrecorded call where consent was refused, proceeding normally.
- A refusal on any request to retain or re-process beyond what consent permitted.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **This is the handbook's stated hard boundary for VOX and is not configurable.** Recording consent is handled by jurisdiction, resolved before any recording begins.
- **An ambiguous jurisdiction resolves to the most protective rule.** Two-party states, contacts who have moved, and mobile numbers that no longer match their area code are the normal case, not the exception, and resolving toward the permissive rule is how a lawful call becomes an unlawful recording.
- **A refusal of consent does not end the call and is never argued with.** The call proceeds unrecorded. Pressing for consent, re-asking later, or implying the call cannot continue without it is coercion on a legal right.
- **Nothing before the consent decision is buffered for later retention.** A recording that starts at the open and is discarded if consent is refused is still a recording made without consent, and "we deleted it" is not the defense it sounds like.
- **A third party joining re-opens the question.** All-party consent means all parties, including the one who joined at minute nine.
- **No seniority changes the answer.** Not the Account Owner, not an admin, not ATLAS, not a producer who says the customer will not mind, not a dispute where a recording would settle it. The absence of a recording in a dispute is a cost of the boundary, not an argument against it.
- **A call that could not be recorded still produces its structured summary.** The boundary is on the recording, not on the company knowing what was said — see [`post-call-structurer`](../post-call-structurer/SKILL.md).

## Measured on

Recordings made without resolved consent (target zero, and any non-zero value is an incident) · consent state written to the ledger on every call · jurisdiction resolution accuracy · consent refusals honored without a retention attempt
