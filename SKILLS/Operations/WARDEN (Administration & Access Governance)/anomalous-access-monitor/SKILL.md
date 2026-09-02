---
name: anomalous-access-monitor
description: Escalates unusual export volume, off-hours logins, and bulk record access, combining WARDEN's own signals with VAULT's document access records. Runs continuously and escalates on detection rather than on a schedule.
agent: WARDEN
division: Operations
binding: mandate
---

# Anomalous Access Monitor

The access that matters is rarely a break-in. It is a legitimate account doing something legitimate at a volume nobody would have approved.

## When this fires

- Continuously, across every account and every access path.
- On unusual export volume, bulk record access, or bulk document retrieval.
- On off-hours or unusual-location logins.
- On a pattern of denied access attempts, which is a signal in its own right.
- On elevated activity from an account whose holder is known to be departing.

## Inputs

- Login, session, and location records.
- Export and bulk-access volume per account against its own baseline.
- Document access and redaction records from VAULT's `role-level-redactor`, including denied attempts.
- Known departures, disputes, and role changes that raise the prior on exfiltration.
- Normal working patterns per role and per person.

## Procedure

1. **Baseline each account against its own normal**, not against an account-wide average.
2. **Watch volume, not just permission.** Every action in a bulk export may be individually permitted; the volume is the signal.
3. **Combine WARDEN's access signals with VAULT's document access records**, including denied and redacted attempts.
4. **Raise the sensitivity for accounts with a known departure, dispute, or role change in progress.**
5. **Escalate on detection**, to the Account Owner, not to the account holder's manager alone where the manager may be involved.
6. **Clock the escalation and name the action available.** State what a revocation would stop and what it would break, so the decision in front of the Account Owner is a decision rather than a notification, and re-escalate rather than letting it age.
7. **State evidence separately from inference.** Unusual is not the same as malicious, and the escalation must not read as an accusation.
8. **Write every detection and every escalation to the audit trail.**

## Output

- An escalation on detection, naming the account, the behavior, and its baseline.
- Evidence separated from inference.
- An audit entry for every detection, including those that resolve to nothing.
- A preservation hold on the records and documents the detection touched, placed with the escalation and released only by a human.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **Volume is the signal, not permission.** Every individual action in a mass export can be fully authorized while the export itself is the incident — a monitor that only looks for permission violations sees nothing at all here.
- **Denied and redacted access attempts are monitored alongside successful ones.** A pattern of attempts against documents someone cannot see is the clearest available signal, and it exists only because VAULT logs denials.
- **A departure in progress raises sensitivity, not suspicion.** The escalation states behavior and baseline; it never characterizes the person, and it never reaches a conclusion WARDEN is not positioned to reach.
- **Escalation goes to the Account Owner.** Routing solely to the account holder's own manager fails in exactly the case where the manager is part of the pattern.
- **The escalation is clocked, names the available action, and re-escalates unacknowledged.** WARDEN escalates rather than suspending — that is the handbook's own division of authority and it is not narrowed here — which means the escalation is the entire response, and an unacknowledged escalation during an export in progress is indistinguishable from never having detected it. An alert that only waits is not a security control.
- **A detection is escalated even when the explanation is probably innocent.** Waiting for certainty converts a monitoring function into a hindsight function.
- **Every detection is recorded, including those that resolve to nothing.** The value of the record is the pattern across months, and a record that only keeps confirmed incidents has no pattern in it.
- **An escalation places a preservation hold on what the detection touched**, and only a human releases it. VAULT's retention schedules dispose of documents on approval and on time; without a hold placed here, the records of a suspected exfiltration are destroyed by a routine, correctly-executed retention run while the investigation is still open.

## Measured on

Security incidents · detection-to-escalation time · access anomalies found after the fact (target zero)
