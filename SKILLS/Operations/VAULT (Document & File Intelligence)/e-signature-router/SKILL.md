---
name: e-signature-router
description: Handles signature routing, reminders, and completion tracking across every party on a document. Fires when a document is ready for signature, on every reminder interval, and on every party action.
agent: VAULT
division: Operations
binding: mandate
---

# E-Signature Router

A signature is a legal act by a specific person. Routing it correctly means the right person, on the right version, with a record of both.

## When this fires

- When a document is ready for signature and its signing parties are identified.
- On each reminder interval for an outstanding signature.
- On every party action — opened, signed, declined, or delegated.
- When a document under signature is superseded by a new version.

## Inputs

- The document, its current version, and the signature fields on it.
- The signing parties, their roles, their order, and their verified contact points.
- The signing deadline and reminder cadence.
- Consent and suppression state for each party's contact channel.

## Procedure

1. **Confirm the document is the current version** before routing anything.
2. **Identify every party and the required order**, and route to the first.
3. **Send reminders on cadence through the party's permitted channel**, respecting suppression and quiet hours the same as any other outbound.
4. **Track and report the real state** — sent, opened, signed, declined — without inferring completion from silence.
5. **Stop the routing immediately if the document is superseded**, and re-route the new version from the start rather than carrying signatures forward.
6. **File the completed document with its full audit record**: who signed, when, from where, on which version.
7. **Escalate a decline or a stall to the human owner** rather than continuing to remind.

## Output

- A routed signature request per party, in order.
- A live completion state per party — never inferred.
- The completed document filed with a full signing audit record.
- An escalation on a decline or a stall.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VAULT — these apply to every VAULT skill, per `AGENTS.md` §5:**

- VAULT **extracts, compares, and flags what changed** between versions — it **never performs legal review** and never advises on the meaning or enforceability of any agreement.
- A **low-confidence extraction is flagged, never guessed** into a field as if it were certain.
- Access control is enforced **at the field level** — a role never sees a field its permissions don't cover.

**Specific to this skill:**

- **A superseded document stops its signature routing immediately, and signatures already collected do not carry forward to the new version.** A signature is consent to a specific text; migrating it to an amended one manufactures agreement that never happened.
- **Signature reminders are outbound messages and are treated as such** — consent, opt-out, and quiet hours apply exactly as they do to any campaign, and a pending signature is not an exemption to any of them.
- **Frequency suppression is where a signature reminder is the send that survives, not the one that yields.** RELAY suppresses the lower-priority send when a contact's touch limit would be breached; a transactional reminder on a document a borrower is waiting to sign is not what yields to a newsletter. Consent and quiet hours are absolute and stop the reminder outright; a frequency rule reorders the queue and must never be the reason a closing slipped.
- **Completion is recorded from a real signing event, never inferred from silence, an open, or a download.**
- **VAULT routes signatures; it never explains what is being signed.** A party asking what a clause means is routed to a human, because answering is legal advice and `legal-review-interlock` forbids it.
- **The signing audit record is complete and permanent** — who, when, from where, and on which version — and it is never subject to retention disposal ahead of the document itself.
- **A decline is escalated, never re-reminded.** Continuing to chase someone who has declined converts a clear answer into harassment.

## Measured on

Signature completion rate · time to completion · signatures carried across versions (target zero)
