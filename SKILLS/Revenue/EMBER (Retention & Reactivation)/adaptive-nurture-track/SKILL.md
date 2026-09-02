---
name: adaptive-nurture-track
description: Runs long-horizon sequences that respond to engagement instead of marching through a fixed calendar. Fires on a contact entering a track and on every engagement signal that should change its path.
agent: EMBER
division: Revenue
binding: mandate
---

# Adaptive Nurture Track

A fixed sequence sends message four to someone who answered message two, and that is how a nurture track becomes an unsubscribe.

## When this fires

- On a contact entering a nurture track.
- On every engagement or disengagement signal against a track in progress.
- On a track running out of genuine content for a contact.

## Inputs

- The contact's track, its position, and its remaining content.
- Engagement signals — opens, replies, clicks, calls answered — with auto-replies excluded.
- The twenty-one-day window state and channel consent.
- Content availability from QUILL and CANVAS, cleared through AEGIS.

## Procedure

1. **Branch on real engagement**, advancing, slowing, pausing, or exiting the track according to what the contact actually did.
2. **Exclude auto-replies from engagement entirely** — an out-of-office is not interest, and reading it as one restarts a cadence at someone on holiday.
3. **Escalate a genuine reply out of the track to ECHO immediately**, rather than continuing the sequence around a live conversation.
4. **Slow the track on sustained non-engagement** rather than continuing at pace, and exit it entirely at the configured floor.
5. **Check the twenty-one-day window before every step**, since a track is dormant-pool outreach.
6. **Pause the whole track on any distress, complaint, or open escalation** on the contact.
7. **Stop the track rather than reuse content** when it runs out of something genuine to say.

## Output

- A track that advances, slows, pauses, or exits on real engagement.
- An immediate handoff to ECHO on a genuine reply.
- A stopped track where no genuine content remains.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **A reply exits the track and goes to ECHO in the same step.** A sequence that keeps sending around a live conversation is the clearest possible signal that nobody is actually there, and it is the most common failure of automated nurture.
- **Auto-replies are never engagement.** ECHO classifies them as their own intent for this reason, and a track that counts them re-engages people who are away and inflates every metric that would have caught it.
- **Sustained non-engagement slows and then exits the track.** Continuing at pace into silence is what drives the unsubscribe rate, which is the metric with a hard number attached to it.
- **Any distress, complaint, or open escalation pauses the track immediately.** A nurture message landing during an open complaint is the detail that gets screenshotted.
- **A track with nothing genuine left to say stops.** Recycled content in a long-horizon track is worse than silence, because it tells the contact exactly how automated the relationship is.
- **Every step is dormant-pool outreach and is subject to the twenty-one-day window**, whatever the track's own schedule says.
- **Any step carrying a rate, payment, term, or cost figure routes through [`figure-disclosure-route`](../figure-disclosure-route/SKILL.md)**, and the figure is never dropped to keep the step simple.

## Measured on

Unsubscribe rate under 0.3% · replies escalated out of the track within the step (must be 100%) · steps sent after a distress signal (target zero) · engagement-driven branches as a share of all branches
