---
name: cross-campaign-suppression
description: Prevents one contact from receiving multiple unrelated sends in a day across RELAY's campaigns, and passes every send to AEGIS's gate for the platform-wide cap. Runs before every send, records after every send, and fails closed.
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

- Per-contact send history across **every campaign on the surface** — email, SMS, and custom campaigns alike, so two uncoordinated campaigns cannot each reach the same contact in a day.
- **AEGIS's platform-wide frequency verdict for the send.** The count that spans BEACON direct messages, EMBER outreach, VOX calls, and SCOUT follow-ups is held at the gate, not here.
- The account's frequency rules per channel and per window — **the ceilings are AEGIS's, read from `quiet-hours-clock`, never a second set maintained here.**
- Campaign priority, for deciding which send yields.
- **AEGIS's consent suppression state, as a separate input from a separate store.**
- Pending and in-flight sends across RELAY's campaigns, so concurrency within the surface is visible.
- The clearance state of the content each pending send carries — its AEGIS gate result, and its CANVAS review result where it has a visual component.

## Procedure

1. **Verify the content's clearance is present and current** before anything else. The send carries an AEGIS pass, and a CANVAS pass where it has a visual component, and neither has since been withdrawn. RELAY runs the send; it does not clear the content, and it never releases content whose clearance it cannot confirm.
2. **Read the contact's total send count across all campaigns on the surface** for the window. One campaign's own sends are not the count.
3. **Apply the frequency rule** for the channel and window — AEGIS's ceiling, against RELAY's campaign-surface count. RELAY supplies the count and the yield decision; it never sets or relaxes the ceiling it is counting against, and it never stands in for the platform-wide count AEGIS applies at the gate.
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
- **Cross-campaign suppression is enforced within the campaign surface** — no contact receives multiple unrelated sends in a day across RELAY's campaigns. Platform-wide frequency caps are AEGIS's, not RELAY's.
- Carrier and messaging compliance registration is maintained continuously. A campaign is **never sent through a lapsed registration**, including to force deliverability against a deadline.
- Clearing RELAY's own campaign suppression is **never a substitute for AEGIS's pre-send gate** — consent, quiet hours, and platform-wide frequency caps are checked there, on every send.

**Specific to this skill:**

- **RELAY never releases content it cannot confirm was cleared.** The AEGIS gate and CANVAS's review clear content; RELAY runs the send. A send whose clearance is absent, unreadable, or stale does not go out — RELAY is the last checkpoint before a message leaves the company, and a checkpoint that assumes the upstream gates fired is not one.
- **A withdrawn or newly blocked asset stops its scheduled send.** RELAY holds the send and tells the campaign owner what was withdrawn; it never releases on the clearance the content held yesterday. QUILL and CANVAS both undertake to notify RELAY before a send — RELAY's own check at send time is what makes that undertaking safe when the notice is late, lost, or never built.
- **Frequency suppression and consent suppression are separate stores with separate lifetimes, and neither is ever derived from the other.** A frequency window expires by design. An opt-out never expires. Deriving one from the other, or storing them together, means a rolling window can undo an opt-out — and that failure is silent, systematic, and applies to every contact at once.
- **This count spans every campaign on the surface; the platform-wide count spans every agent and is AEGIS's.** Two RELAY campaigns each sending once in a day is two sends, and a per-campaign count is not a cap. The count that also takes in a BEACON direct message, an EMBER outreach, or a VOX call is applied at the AEGIS gate every send passes through — RELAY does not keep a second one, because a roster-wide rule enforced in two places is enforced twice or not at all.
- **A transactional send is never the one that yields to a marketing send.** An appointment confirmation or reminder, a document request on a live file, a signature reminder, and a no-show recovery are responses to something the contact did or booked — a newsletter is not a reason a customer misses the appointment they booked or the closing they were waiting to sign for. TEMPO's `booking-lifecycle`, FORGE's `vault-first-chaser`, and VAULT's `e-signature-router` each state this from their own side and depend on it holding here. It governs ordering only: a frequency rule reorders the queue, while consent, permanent exit, and quiet hours stop a transactional send outright and are never reordered around.
- **A voicemail drop from a RELAY voice campaign is a send on this surface and is counted here; the roster-wide voicemail cap is not this skill's.** VOX's `voicemail-drop-governor` reads AEGIS's one-per-contact-per-day drop counter before every drop, and AEGIS enforces it. Both rules apply to a campaign-triggered drop, and clearing this suppression is not clearance to drop — neither check substitutes for the other, and neither agent assumes the other ran.
- **A send to a dormant contact honors EMBER's twenty-one-day next-touch window.** The window is EMBER's `21-day-collision-guard` and RELAY observes it, as a third rule alongside this suppression and the AEGIS gate. None of the three stands in for the others.
- **A suppressed send is recorded as suppressed and its owner is told.** A send that silently disappears looks identical to one that was never scheduled, and the campaign it belonged to reports on an audience it never reached.
- **Suppression is never budget-gated, never deferred to the next cycle, and never skipped under load.** It is a compliance-critical operation and proceeds regardless of any ceiling — a contact who keeps receiving messages because a ledger ran out of tokens is the worst outcome the send path can produce.
- **If suppression state cannot be read, nothing sends.** RELAY fails closed and declares the halt to ATLAS with the full scope of what is stopped, immediately. Failing closed without an alarm is indistinguishable from work quietly not happening.
- **A RELAY halt never blocks a consent-honoring message.** An opt-out confirmation, a suppression acknowledgement, or any message AEGIS must send to honor a consent request is exempt from every RELAY-side stop — this one included. Where the infrastructure genuinely cannot carry it, the obligation does not lapse: the failure is declared immediately to AEGIS and the Account Owner as an incident, naming the contacts whose confirmations are outstanding.
- **Recovery order is fixed: suppression and consent backlog first, send queue second.** It is never reversed to clear a backlog faster.

## Measured on

Complaint and bounce rates · delivery rate · cost per engaged contact
