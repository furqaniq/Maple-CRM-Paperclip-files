---
name: permission-auditor
description: Audits the permission model and flags over-privileged accounts and access that no longer matches someone's job. Fires on the scheduled audit, on every role change, and whenever access drifts from its role definition.
agent: WARDEN
division: Operations
binding: mandate
---

# Permission Auditor

Permissions only ever grow. This is the skill that makes them shrink.

## When this fires

- On the scheduled permission audit.
- On every role change, transfer, or promotion.
- When an account's access diverges from its role definition.
- When [`anomalous-access-monitor`](../anomalous-access-monitor/SKILL.md) surfaces behavior a role should not have made possible.

## Inputs

- Every account with its granted access, compared against its role definition.
- Actual usage per permission — what each account has exercised versus what it holds.
- Role changes and transfers, with what was added and what was never removed.
- The set of permissions that would allow bulk export, bulk messaging, or access to sensitive fields — including whose compensation figures an account may see.

## Procedure

1. **Compare every account's granted access against its role definition**, and flag every divergence.
2. **Flag accumulated access from transfers** — the permissions someone kept from their previous role and no longer needs.
3. **Compare held permissions against exercised ones.** A permission held for a year and never used is a standing risk with no offsetting benefit.
4. **Rank findings by blast radius**, not by count. One account that can bulk-export the contact database outranks forty that can see an extra report.
5. **Propose the specific removal**, with what it would stop the person doing.
6. **Execute on approval**, and write the audit entry.
7. **Report the findings whether or not the over-privileged account belongs to someone senior.**

## Output

- An audit finding per divergence, ranked by blast radius.
- A proposed removal per finding, with its practical effect stated.
- An audit entry for every change executed.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **Access accumulated through a transfer is a finding, not a grandfathered right.** The permission set someone carries out of a previous role is the largest single source of over-privilege, and nobody notices because nothing broke.
- **Findings are ranked by blast radius, never by count.** A report sorted by number of findings buries the one account that can export everything under forty trivial ones.
- **An over-privileged senior account is reported identically to any other.** The accounts with the widest access belong to the people it is most uncomfortable to audit, which is precisely why the audit cannot be discretionary.
- **WARDEN proposes removals and executes them on approval**, never removing access unilaterally on audit findings alone — an unapproved removal during working hours breaks someone's day and teaches the organization to distrust the audit.
- **A permission held and never exercised is reported as a finding.** Unused access carries the full risk of used access and none of the benefit.
- **Every removal is written to the audit trail** with what was removed, from whom, and why.
- **Compensation visibility is part of the permission model WARDEN owns.** A role definition states whose compensation figures an account may see — their own, their team's, their branch's — because TALLY's producer view defers that decision here rather than making it. A model that has no answer for compensation leaves TALLY to choose, and one file's splits, overrides, and referral fees involve several people who were never meant to see each other's numbers.

## Measured on

Permission audit findings · over-privileged accounts outstanding · access removed versus access accumulated per period
