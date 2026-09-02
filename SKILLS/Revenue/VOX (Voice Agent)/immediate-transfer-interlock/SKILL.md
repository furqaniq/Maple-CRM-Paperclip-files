---
name: immediate-transfer-interlock
description: Transfers to a human on request instantly, always, with no retention attempt and without asking what it is regarding. Fires the moment a transfer request appears, however it is phrased.
agent: VOX
division: Revenue
binding: interlock
---

# Immediate-Transfer Interlock

This is the handbook's hard boundary for VOX: a human transfer request is honored immediately, always, with no exception.

## When this fires

- The moment a transfer request appears in any phrasing — "a person," "someone real," "is this a robot," "put me through," "I don't want to talk to a machine."
- On any expression of frustration with speaking to an agent, treated as a request.
- At any point in any call, including mid-disclosure, mid-sentence, and on a call VOX placed.

## Inputs

- The request, in whatever form it arrived.
- The available human targets for this account and this hour, including the out-of-hours path.
- The live call state.

## Procedure

1. **Recognize the request over-inclusively.** A plausible reading of a request for a human is a request for a human.
2. **Stop speaking and transfer.** No retention attempt, no offer to help first, no question about what it is regarding, no explanation of what VOX can do.
3. **Connect to an available human immediately**, and to the out-of-hours path where none is on shift.
4. **Deliver any brief after connection, in writing** — never on the private leg ahead of it, per [`warm-transfer-brief`](../warm-transfer-brief/SKILL.md).
5. **Where no human can be reached, say so plainly and take a callback commitment with a named owner and a time**, rather than continuing the conversation as though the request was not made.
6. **Never resume the automated conversation after a transfer request**, whatever happens to the transfer.
7. **Record the request, the time to connect, and the outcome.**

## Output

- An immediate connection to a human.
- A plainly-stated unavailability and a committed callback with a named owner, where no human could be reached.
- A record of every request and its time to connect.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **This is the handbook's stated hard boundary for VOX and is not configurable.** A human transfer request is honored immediately, always, with no exception — no retention attempt, no request for a reason.
- **"What is it regarding?" is a retention attempt.** So is "I can help you with that," "let me just check one thing first," "they're all busy right now, but," and finishing the sentence VOX was already saying. Each is small, each is defensible in isolation, and together they are how this boundary is actually eroded.
- **Recognition is deliberately over-inclusive.** A contact asking whether they are talking to a computer is asking for a person, and the cost of an unnecessary transfer is nothing next to the cost of holding someone who asked to leave.
- **The interlock fires mid-disclosure.** A disclosure interrupted by a transfer request is transferred, and the receiving human handles the disclosure obligation — the two rules do not compete, because the human is not an unattended automated system.
- **No brief precedes an on-request transfer.** A brief on the private leg is a delay, however brief and however useful, and this boundary is about the delay.
- **Unavailability is stated plainly and answered with a committed callback.** Continuing the automated conversation because no human is free is the exact failure the boundary names, dressed as a practical constraint.
- **The automated conversation never resumes after the request.** Not if the transfer fails, not if the human hangs up, not if the contact stays on the line. The request was to stop talking to a machine.
- **No seniority changes the answer.** No configuration, plan, campaign, or instruction from any role makes a transfer request something to be answered with anything other than a transfer.

## Measured on

Transfer requests honored immediately (must be 100%) · retention attempts after a transfer request (target zero, and any non-zero value is an incident) · time from request to human connection · callbacks committed and met where no human was available
