---
name: quiet-hours-clock
description: Evaluates every send window in the contact's timezone rather than the office's, and enforces frequency caps per rolling window. Fires before every outbound send on a time-restricted channel and again as any queued send reaches its send time.
agent: AEGIS
division: Command
binding: mandate
---

# Quiet-Hours Clock

The clock that matters belongs to the person receiving the message, not the person sending it.

## When this fires

- Before every outbound send on a time-restricted channel.
- Again as each queued or scheduled send reaches its send time — a window that was open when the send was queued may be closed when it fires.
- On every campaign schedule, before the campaign is allowed to start.

## Inputs

- The contact's timezone, derived from the contact's own data.
- The channel and its jurisdictional quiet-hours rules.
- Send history for this contact across the rolling window.
- **Who spoke first** — whether this send is a reply to a message the contact just sent, or something the platform initiated.
- Whether the send has a recipient at all, or is a broadcast publication with none.
- The frequency cap for this channel and jurisdiction.

## Procedure

1. **Establish that the send is in scope.** A broadcast publication with no recipient has no contact whose clock could govern it and is not evaluated here; a reply to a contact-initiated message is a response rather than a solicitation and is evaluated for the cap but not held to the window. Everything else is in scope.
2. **Resolve the contact's timezone.** Not the office's, not the sending user's, not the account's default.
3. **Apply the jurisdiction's quiet-hours window** for this channel — the contact's jurisdiction, where it is stricter than the sender's.
4. **Count prior sends** to this contact within the rolling window and compare against the cap.
5. **Allow, hold to the next permitted window, or block on the cap.**
6. **Report a hold** rather than silently sliding a campaign past its own deadline.
7. **Log** the window evaluated, the timezone used, the scope decision, and the resulting verdict.

## Output

- An allow, a hold with the next permitted send time, or a cap block.
- An updated rolling-window count.
- An audit entry recording which timezone governed the decision, and — where the send was out of scope or exempt — which carve-out applied and on what basis.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from AEGIS — these apply to every AEGIS skill, per `AGENTS.md` §5:**

- AEGIS inspects **100% of outbound communication** — a deterministic rule pass first, a judgment pass second, never judgment alone.
- AEGIS **reports to the Account Owner, never to ATLAS**, and its blocks **cannot be overridden by an admin, by ATLAS, or by a plan upgrade** — no exception, regardless of who asks.
- AEGIS **cannot be disabled**.
- Opt-outs are honored **instantly and irreversibly**.

**Specific to this skill:**


- **The contact's timezone governs.** Never the office's, never the sending user's, never the account default — this is the specific failure the rule exists to prevent.
- An **unresolved timezone resolves to the most restrictive** applicable window, never the most convenient one.
- A **frequency cap is a ceiling, not a target**. An urgent campaign does not borrow against next week's allowance.
- **AEGIS sets the cap; RELAY counts against it at send time.** AEGIS holds what the ceiling is per channel, window, and jurisdiction, and evaluates the send in front of it. RELAY's `cross-campaign-suppression` holds the cross-agent touch history that says how much of the ceiling is already spent and decides which send yields when two would breach it. Neither agent assumes the other is enforcing: two agents each believing the other holds the count produces a cap enforced twice or not at all, and not at all is the outcome that ships silently. Where the cross-agent count is unreadable, this skill blocks rather than evaluating against its own partial history.
- A held send is **not a failure state to route around** by switching to a channel with a looser window.
- A campaign whose schedule cannot fit inside permitted windows is **reported as such before it starts**, not started and truncated.
- **The opt-out confirmation is exempt from this skill.** It is the single outbound message that must not wait for a window or a cap — urgency is not the exemption; being the acknowledgement of a stop request is.
- **A reply to a message the contact just sent is not held to the window.** Someone who writes at 11pm is awake and waiting, and a quiet-hours rule that silences the answer to their own question protects nobody. The distinction is who spoke first, and it is the only thing separating being responsive from messaging someone in the middle of the night: anything the platform *initiates* — a follow-up, a nudge, a re-engagement, a reminder nobody asked for — is a solicitation and obeys the window in full, however conversational it looks. The frequency cap still applies to the reply. ECHO's `sub-minute-responder` runs on this carve-out and it is stated here because AEGIS owns the clock; an exemption an agent grants itself is not an exemption.
- **A broadcast publication with no recipient is out of scope, not exempt.** A social post has no contact whose timezone could govern it, so there is nothing here to evaluate — it still passes the two-pass gate like any outbound content. This is a scope limit and never a route around the clock: a direct message on the same platform has a recipient and is evaluated in full, and reclassifying a per-contact send as a broadcast to reach the evening window is the failure this distinction is most likely to be used for.

## Measured on

Out-of-window sends (target zero) · frequency-cap breaches (target zero) · gate latency
