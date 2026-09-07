---
name: voicemail-drop-governor
description: Detects voicemail, leaves the approved message, and reads the roster-wide drop counter before every drop so the one-per-contact-per-day cap holds across every agent. Fires on every voicemail detection during a VOX call.
agent: VOX
division: Revenue
binding: mandate
---

# Voicemail Drop Governor

One voicemail a day, counted across the whole roster — because three agents each leaving one is three voicemails, whatever each of them believed.

## When this fires

- On voicemail detection during any VOX call, inbound or outbound.
- Before any VOX drop, to read the roster-wide counter for this contact.
- On a VOX call originating from a RELAY voice campaign, an EMBER re-engagement, or a TEMPO reminder — the trigger differs, the check does not.

## Inputs

- The voicemail detection and the call that produced it.
- The roster-wide drop counter for this contact, for the current day in the contact's timezone. AEGIS holds this counter; this skill reads it.
- AEGIS's pre-send verdict for the drop — consent, quiet hours, and frequency are decided there.
- RELAY's campaign-surface suppression state, where the call originated in a RELAY campaign.
- Exit state and the permissible calling window.
- The approved voicemail script from QUILL's `script-writer`, with its required disclosures.

## Procedure

1. **Detect voicemail reliably** and distinguish it from a live answer before dropping anything.
2. **Read the roster-wide counter before every drop.** The counter is AEGIS's; VOX reads it and keeps no count of its own.
3. **Pass the drop through AEGIS's pre-send gate**, which decides frequency across the platform — including whether the day's single message has already been left by another agent.
4. **Do not drop where the counter or the gate refuses**, and report the refusal to whatever triggered the call, naming the agent that consumed the day's message.
5. **Leave the approved script**, including the disclosures the jurisdiction requires in a recorded message.
6. **Report the drop to AEGIS at the moment it is left**, so a concurrent read by another agent sees it.
7. **Record the drop, its script version, and the originating trigger.**

## Output

- A left voicemail from the approved script, with disclosures included.
- A recorded non-drop where the counter or the gate refused, naming the agent that consumed the cap, returned to the trigger rather than dropped silently.
- A drop notification to AEGIS and a local drop record.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.
- Outbound calls and voicemails pass **AEGIS's pre-send gate** for consent, quiet hours, and frequency — speed-to-lead urgency is never a reason to skip it.

**Specific to this skill:**

- **The cap is one per contact per day across every agent, and the counter that carries it is AEGIS's.** VOX reads it before every drop and reports every drop to it. VOX keeps no count of its own and does not gate another agent's drop — contact-frequency enforcement across the platform has one enforcement point, and two agents each enforcing a roster-wide rule is how it ends up enforced twice or not at all.
- **A per-agent cap is not a cap.** It is several independent caps that add up to whatever the roster size happens to be, which is why the count VOX reads is the roster's and not its own.
- **RELAY's suppression is a different rule with a different scope, and both apply.** RELAY holds suppression within the campaign surface; the platform-wide frequency cap is AEGIS's. Clearing RELAY's suppression is not clearance to drop, and neither check substitutes for the other.
- **A drop to a dormant contact honors EMBER's twenty-one-day next-touch window.** The window is EMBER's `21-day-collision-guard` and VOX observes it rather than counting for itself — a re-engagement drop is dormant-pool outreach whichever agent triggered the call.
- **The counter is read immediately before the drop and the drop is reported at the moment it is left**, so two agents dropping within the same minute cannot both pass.
- **A refusal is reported, never silent.** A trigger that thinks its message was left will not retry through another channel, and the contact hears nothing at all.
- **The day boundary is the contact's timezone.** An account-timezone reset gives a contact two messages inside eight hours across a date line and calls it compliant.
- **A voicemail is an outbound message and carries every required disclosure.** A recorded message is the least deniable form of outbound content the company produces.
- **No drop on an exited or unconsented channel, and none outside the permissible calling window.** A voicemail left at 6am is a call placed at 6am.
- **Only the approved script is left.** An improvised voicemail is an unreviewed outbound recording with the company's name on it.

## Measured on

Voicemail drops per contact per day (cap: one, across all agents) · drops made without reading the roster-wide counter (target zero) · drops exceeding the cap (target zero) · unreported refusals (target zero) · drops outside the permissible window (target zero)
