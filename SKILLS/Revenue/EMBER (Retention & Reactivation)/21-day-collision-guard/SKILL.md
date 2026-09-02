---
name: 21-day-collision-guard
description: Prevents any agent from touching the same dormant contact twice inside the twenty-one-day window, counting every agent rather than EMBER's own outreach. Runs before every dormant-pool touch and records after every one.
agent: EMBER
division: Revenue
binding: mandate
---

# 21-Day Collision Guard

Twenty-one days counted across the whole roster — because four agents each respecting their own window is four touches in a month.

## When this fires

- Before every dormant-pool touch, from any agent.
- After every such touch, to record it against the window.
- On a request to override the window.

## Inputs

- The proposed touch, its channel, and the agent proposing it.
- The cross-agent touch history for this contact inside the window.
- Whether the proposed touch is dormant-pool outreach or a transactional response.
- RELAY's `cross-campaign-suppression` state, which holds the separate per-day cap.

## Procedure

1. **Classify the proposed touch as dormant-pool outreach or transactional**, because only the first is in scope.
2. **Check the cross-agent history for the window** before the touch, counting every agent's outreach and not only EMBER's.
3. **Suppress the later, lower-priority touch where the window would be breached**, and tell the proposing agent it was suppressed and why.
4. **Record every touch against the window at send time**, so a concurrent proposal from another agent sees it.
5. **Fail closed** — where the history cannot be read, the touch does not go.
6. **Refuse override requests** and record them.
7. **Report suppression patterns**, so a contact whose outreach is repeatedly suppressed is visible as a coordination problem rather than as silence.

## Output

- A pass or a suppression on every proposed dormant-pool touch, with the reason returned to the proposing agent.
- The recorded touch history for the window.
- A suppression-pattern report where one contact is repeatedly contested.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from EMBER — these apply to every EMBER skill, per `AGENTS.md` §5:**

- Any message with a **rate, payment, term, or cost figure** routes through **AEGIS's disclosure builder automatically** — EMBER never drops the number to dodge the rule.
- Review requests are asked **once, without incentives**, and an unhappy customer is **never routed away from public platforms**.
- No dormant contact is touched twice inside the **21-day window**, regardless of which agent already reached out.

**Specific to this skill:**

- **Twenty-one days, counted across every agent — this is EMBER's own boundary.** A per-agent window is not a window; it is several independent ones that add up to however many agents the account runs.
- **The guard covers dormant-pool outreach, never a transactional response.** An appointment reminder, a no-show recovery within minutes, a document request on a live file, a reply to a message the contact just sent, and a milestone update are responses to something the contact did or booked. Applying a twenty-one-day silence to them would mean TEMPO's no-show recovery never fires and FORGE never chases a document twice in a month — which is not caution, it is the guard breaking the operations it was never meant to touch. TEMPO's `no-show-recovery` states the same carve-out from its side.
- **Transactional is a carve-out from frequency, never from consent or exit state.** Both apply in full to everything.
- **This window and RELAY's per-day cap are two different rules and both apply.** RELAY holds the cross-agent daily cap; this holds the dormant-pool window. Neither agent assumes the other is enforcing, because the outcome of that assumption is a cap enforced twice or not at all, and not at all is the one that ships silently.
- **The guard fails closed.** An unreadable history is not permission; it is the moment when a contact is most likely to receive four messages.
- **A suppression is reported to the proposing agent, never silent.** An agent that believes its message went out will not find another way to reach the contact, which is the outcome the guard actually wants.
- **The window is not overridable.** Not for a campaign, not for an anniversary, not for a market event, not by an admin. A window with an override is a suggestion.

## Measured on

Contacts touched twice inside the window (target zero) · suppressions reported to the proposing agent (must be 100%) · transactional sends wrongly suppressed (target zero) · override requests, all refused
