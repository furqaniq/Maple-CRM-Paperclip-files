---
name: voicemail-drop-governor
description: Detects voicemail, leaves a message, and holds the one-per-contact-per-day cap across every agent rather than VOX's own drops alone. Fires on every voicemail detection and on every drop request from any agent.
agent: VOX
division: Revenue
binding: mandate
---

# Voicemail Drop Governor

One voicemail a day, counted across the whole roster — because three agents each leaving one is three voicemails, whatever each of them believed.

## When this fires

- On voicemail detection during any VOX call.
- On a voicemail drop request from any other agent — a RELAY voice campaign, an EMBER re-engagement, a TEMPO reminder.
- At the daily boundary, when the counter resets in the contact's own timezone.

## Inputs

- The voicemail detection or the drop request, and its originating agent.
- The cross-agent drop counter for this contact, for the current day in the contact's timezone.
- The contact's cross-agent touch count from RELAY's `cross-campaign-suppression`, which is a separate rule with a separate scope.
- Consent, exit state, and the permissible calling window.
- The approved voicemail script from QUILL's `script-writer`, with its required disclosures.

## Procedure

1. **Detect voicemail reliably** and distinguish it from a live answer before dropping anything.
2. **Check the cross-agent counter before every drop**, whatever agent requested it.
3. **Refuse the drop where the day's single message has already been left by any agent**, and tell the requesting agent it was refused rather than failing quietly.
4. **Leave the approved script**, including the disclosures the jurisdiction requires in a recorded message.
5. **Increment the shared counter at the moment of the drop**, so a concurrent request from another agent sees it.
6. **Reset the counter at the day boundary in the contact's timezone**, not the account's.
7. **Record the drop, its script version, and the requesting agent.**

## Output

- A left voicemail from the approved script, with disclosures included.
- A refusal to the requesting agent where the cap was already consumed, naming the agent that consumed it.
- The updated cross-agent counter and a drop record.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **The cap is one per contact per day across every agent, and this skill owns the counter for all of them.** A per-agent cap is not a cap — it is several independent caps that add up to whatever the roster size happens to be. Every agent that can leave a voicemail asks here first, and no agent keeps its own count.
- **This counter and RELAY's cross-agent touch count are two different rules and both apply.** This skill holds the voicemail cap; RELAY's `cross-campaign-suppression` holds the total touch count across every channel, and a voicemail is one of those touches. Clearing this counter is not clearance to drop — the touch count is checked as well, and either can refuse. Neither agent assumes the other is enforcing, because the outcome of that assumption is a cap enforced twice or not at all, and not at all is the one that ships silently.
- **The counter is checked immediately before the drop and incremented at the drop**, so two agents requesting within the same minute cannot both pass.
- **A refusal is reported to the requesting agent, never silent.** An agent that thinks its message was left will not retry through another channel, and the contact hears nothing at all.
- **The day boundary is the contact's timezone.** An account-timezone reset gives a contact two messages inside eight hours across a date line and calls it compliant.
- **A voicemail is an outbound message and carries every required disclosure.** A recorded message is the least deniable form of outbound content the company produces.
- **No drop on an exited or unconsented channel, and none outside the permissible calling window.** A voicemail left at 6am is a call placed at 6am.
- **Only the approved script is left.** An improvised voicemail is an unreviewed outbound recording with the company's name on it.

## Measured on

Voicemail drops per contact per day (cap: one, across all agents) · drops exceeding the cap (target zero) · silent refusals to requesting agents (target zero) · drops outside the permissible window (target zero)
