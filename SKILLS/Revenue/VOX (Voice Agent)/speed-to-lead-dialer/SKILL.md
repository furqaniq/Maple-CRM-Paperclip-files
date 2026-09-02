---
name: speed-to-lead-dialer
description: Places the outbound call the moment SCOUT fires an intake event, inside the contact's own permissible calling window. Fires on every intake event carrying phone consent.
agent: VOX
division: Revenue
binding: mandate
---

# Speed-to-Lead Dialer

The lead that gets called in ninety seconds and the one called the next morning are, statistically, two different businesses.

## When this fires

- On every intake event from SCOUT's `instant-intake-event` carrying consent to call.
- On a retry where the first attempt did not connect, inside the configured attempt limit.
- Never on an intake event for a held-for-review identity or a junk rejection.

## Inputs

- The intake event, with per-attribute state marking what is still pending.
- Consent and exit state per channel, carried in the event itself.
- The permissible calling window for the contact's own jurisdiction and timezone, from AEGIS's `quiet-hours-clock`.
- The assignment from SCOUT's `capacity-aware-router`, for the warm transfer target.

## Procedure

1. **Verify phone consent and exit state from the event** before dialing, without a second lookup that would cost the window.
2. **Check the permissible calling window in the contact's own timezone**, and queue to the window's opening rather than dialing outside it.
3. **Dial immediately** where the window is open.
4. **Deliver the disclosure at call open and resolve recording consent**, exactly as on an inbound call.
5. **Work from the record's confirmed attributes only**, treating anything the event marked pending as absent for the purpose of what is said.
6. **Hand to [`voicemail-drop-governor`](../voicemail-drop-governor/SKILL.md) on voicemail detection**, rather than leaving a message directly.
7. **Stop at the configured attempt limit** and hand to ECHO's written channels rather than continuing to dial.

## Output

- A placed call with the disclosure delivered and consent resolved, or a queued call held to the window's opening.
- A voicemail decision routed through the governor.
- A handoff to written channels where the attempt limit was reached.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **Speed never overrides the calling window.** The window is the contact's own jurisdiction and timezone, and a lead that arrives at 11:40pm local is called when the window opens. Every part of this skill is built for speed except this, and this is the part that carries the statutory damages.
- **Consent travels in the event and is checked before the dial.** A speed-to-lead path that skips the check to save the lookup is a path that calls people who opted out, and it will do so fastest of all.
- **Pending attributes are treated as absent on the call.** A position estimate still being fetched is not a thing to mention, and a figure that lands mid-call is not a correction to say out loud.
- **The disclosure and recording consent are handled identically on outbound and inbound.** There is no abbreviated opening for a speed-to-lead call.
- **The attempt limit is a limit.** Four dials in an afternoon is harassment regardless of how the record was sourced, and the handoff to written channels exists so the limit costs nothing.
- **No approval, denial, pre-approval, or eligibility outcome is stated**, and no rate, payment, term, or cost figure is quoted without the required disclosures. A first call is where an offhand number does the most damage.
- **A held-for-review identity is never dialed.** Calling one of two unresolved records means discussing one person's situation with another.

## Measured on

Median time from intake event to first dial · calls placed outside the permissible window (target zero) · calls placed on an exited or unconsented channel (target zero) · connect rate and conversion to appointment
