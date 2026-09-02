---
name: cross-campaign-suppression
description: Prevents one contact from receiving multiple unrelated sends in a day, counting every agent that reaches them rather than RELAY's campaigns alone. Runs before every send, records after every send, and fails closed.
agent: RELAY
division: Marketing
binding: mandate
---

# Cross-Campaign Suppression

Frequency windows expire. Opt-outs do not. Anything that lets the first mechanism resurrect the second is the worst bug this company can ship.

## When this fires

- Before every send, at send time, for every contact.
- After every completed send, to record it immediately.
- On the scheduled reconciliation, for sends recorded late or through another path.
- On a content withdrawal affecting a scheduled send — a QUILL template retirement, a CANVAS expiry or rights withdrawal, an AEGIS block.
- On recovery from any outage, before the send queue is touched.

## Inputs

- Per-contact touch history across **every campaign and every agent** — RELAY sends, BEACON direct messages, EMBER outreach, VOX calls, SCOUT follow-ups.
- The account's frequency rules per channel and per window — **the ceilings are AEGIS's, read from `quiet-hours-clock`, never a second set maintained here.**
- Campaign priority, for deciding which send yields.
- **AEGIS's consent suppression state, as a separate input from a separate store.**
- Pending and in-flight sends from every agent, so concurrency is visible.
- The clearance state of the content each pending send carries — its AEGIS gate result, and its CANVAS review result where it has a visual component.

## Procedure

1. **Verify the content's clearance is present and current** before anything else. The send carries an AEGIS pass, and a CANVAS pass where it has a visual component, and neither has since been withdrawn. RELAY runs the send; it does not clear the content, and it never releases content whose clearance it cannot confirm.
2. **Read the contact's total touch count across all campaigns and all agents** for the window. RELAY's own sends alone are not the count.
3. **Apply the frequency rule** for the channel and window — AEGIS's ceiling, against RELAY's cross-agent count. RELAY supplies the count and the yield decision; it never sets or relaxes the ceiling it is counting against.
4. **Where the rule would be breached, suppress the lower-priority send**, and record which send yielded and to what. A transactional send — a signature reminder, a document request, a scheduled-appointment confirmation — is never the one that yields to a marketing send.
5. **Record every completed send back to the shared history immediately**, before the next check can run, so a concurrent send by another agent sees it rather than racing it.
6. **Reconcile on schedule** for sends recorded late or by a path outside the normal one.
7. **On recovery from any outage, drain the suppression and consent backlog before the send queue.** A queue released ahead of the suppression state it depends on is a queue sending to people who already said no.

## Output

A per-send suppression decision with the touch count it was made on, the rule applied, and — where a send yielded — which send, to what, and its owner notified; plus an immediately written history record for every send that went out.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from RELAY — these apply to every RELAY skill, per `AGENTS.md` §5:**

- A/B and multivariate winners are declared only on a **real statistical threshold**. An inconclusive test is reported as inconclusive; noise is never dressed as a result.
- **Cross-campaign suppression is enforced** — no contact receives multiple unrelated sends in a day, no matter which campaign or which agent owns each one.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.

**Specific to this skill:**

- **RELAY never releases content it cannot confirm was cleared.** The AEGIS gate and CANVAS's review clear content; RELAY runs the send. A send whose clearance is absent, unreadable, or stale does not go out — RELAY is the last checkpoint before a message leaves the company, and a checkpoint that assumes the upstream gates fired is not one.
- **A withdrawn or newly blocked asset stops its scheduled send.** RELAY holds the send and tells the campaign owner what was withdrawn; it never releases on the clearance the content held yesterday. QUILL and CANVAS both undertake to notify RELAY before a send — RELAY's own check at send time is what makes that undertaking safe when the notice is late, lost, or never built.
- **Frequency suppression and consent suppression are separate stores with separate lifetimes, and neither is ever derived from the other.** A frequency window expires by design. An opt-out never expires. Deriving one from the other, or storing them together, means a rolling window can undo an opt-out — and that failure is silent, systematic, and applies to every contact at once.
- **The touch count spans every agent that reaches the contact.** One email, one BEACON direct message, and one VOX call in a day is three touches. A per-agent count is not a frequency cap, it is several independent ones that add up.
- **A transactional send is never the one that yields to a marketing send.** An appointment confirmation or reminder, a document request on a live file, a signature reminder, and a no-show recovery are responses to something the contact did or booked — a newsletter is not a reason a customer misses the appointment they booked or the closing they were waiting to sign for. TEMPO's `booking-lifecycle`, FORGE's `vault-first-chaser`, and VAULT's `e-signature-router` each state this from their own side and depend on it holding here. It governs ordering only: a frequency rule reorders the queue, while consent, permanent exit, and quiet hours stop a transactional send outright and are never reordered around.
- **A voicemail drop is a touch and is counted here, and VOX's own cap is a second rule that also applies.** VOX's `voicemail-drop-governor` holds the one-per-contact-per-day voicemail counter for every agent; this skill holds the cross-agent touch count across all channels. They are different rules with different scopes and both are checked — a drop that clears VOX's counter can still breach the day's touch count, and neither agent assumes the other is enforcing.
- **A suppressed send is recorded as suppressed and its owner is told.** A send that silently disappears looks identical to one that was never scheduled, and the campaign it belonged to reports on an audience it never reached.
- **Suppression is never budget-gated, never deferred to the next cycle, and never skipped under load.** It is a compliance-critical operation and proceeds regardless of any ceiling — a contact who keeps receiving messages because a ledger ran out of tokens is the worst outcome the send path can produce.
- **If suppression state cannot be read, nothing sends.** RELAY fails closed and declares the halt to ATLAS with the full scope of what is stopped, immediately. Failing closed without an alarm is indistinguishable from work quietly not happening.
- **A RELAY halt never blocks a consent-honoring message.** An opt-out confirmation, a suppression acknowledgement, or any message AEGIS must send to honor a consent request is exempt from every RELAY-side stop — this one included. Where the infrastructure genuinely cannot carry it, the obligation does not lapse: the failure is declared immediately to AEGIS and the Account Owner as an incident, naming the contacts whose confirmations are outstanding.
- **Recovery order is fixed: suppression and consent backlog first, send queue second.** It is never reversed to clear a backlog faster.

## Measured on

Complaint and bounce rates · delivery rate · cost per engaged contact
