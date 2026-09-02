---
name: day-one-provisioner
description: Creates a user with the correct role, branch, territory, and module access on their first day, granted from a defined role rather than copied from a colleague. Fires on a confirmed start date and on any role change.
agent: WARDEN
division: Operations
binding: mandate
---

# Day-One Provisioner

Access granted on day one is the access someone keeps for years. Getting it right once is cheaper than auditing it forever.

## When this fires

- On a confirmed start date, ahead of the first day rather than on it.
- On a provisioning request from HARBOR's `onboarding-orchestrator`, which sequences onboarding and never provisions.
- On a role change for an existing user.
- On a branch or territory transfer.
- When a contractor or temporary seat is created.

## Inputs

- The person, their start date, their role, and their branch — from the hiring record, or from HARBOR's onboarding request naming the role as agreed.
- The role definition — the standard permission set for that job.
- Territory and routing assignment from [`org-structure-manager`](../org-structure-manager/SKILL.md).
- Module entitlements for their branch and seat.
- The licensing state — whether the role requires a licence the person holds.

## Procedure

1. **Grant from the role definition**, never by copying an existing user's access.
2. **Verify the licensing requirement** where the role touches licensed activity, and provision without it only where the role genuinely does not require it.
3. **Assign branch, territory, and routing** from the org structure rather than by hand.
4. **Activate only the modules the role uses**, per [`module-activation-control`](../module-activation-control/SKILL.md).
5. **Set an expiry on any temporary or contractor seat at creation time**, not as a follow-up task.
6. **Write the audit entry**: who was provisioned, by whom, into which role, and why.
7. **Register the seat with ABACUS** so its consumption is attributed from the first day.
8. **Return an explicit acceptance or refusal to the requester**, naming the role provisioned or what is missing. A request that is neither accepted nor refused is a first day with no login that nobody discovers until the person arrives.

## Output

- A provisioned user with role, branch, territory, and module access set.
- An expiry date on any temporary seat.
- An audit entry naming who, when, and why.
- A seat registration delivered to ABACUS.
- An explicit acceptance or refusal to the requesting agent, naming the role provisioned or the missing prerequisite.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **Access is granted from a role definition, never copied from a colleague.** Copying propagates every over-privilege the source account accumulated, and it does so invisibly — the new account looks correctly provisioned because it matches someone who was not.
- **A temporary or contractor seat carries an expiry set at creation.** An expiry added later is an expiry that will not be added, and the account becomes orphaned the day the engagement ends.
- **A role requiring a licence is not provisioned to someone without one.** Provisioning first and verifying later means a licensed activity gets performed unlicensed in the interval.
- **Provisioning never grants more than the role defines, even at the request of the person's manager.** A broader grant is a role change with a role definition behind it, or it does not happen. This holds identically for a request arriving from HARBOR: an onboarding sequence is not an authority to widen a role, and a request built from a colleague's access rather than a role definition is refused rather than trimmed.
- **A provisioning request is answered explicitly, never left pending.** HARBOR confirms every handoff rather than assuming it, and that undertaking is only satisfiable if WARDEN actually answers. Silence on a request due before someone's first day is indistinguishable from a refusal nobody was told about, and it is discovered by the new hire.
- **Every provisioning is written to the audit trail with a stated reason**, with no exception for urgency or seniority.
- **Provisioning ahead of the start date is normal; provisioning without a confirmed start date is not.** A speculative account is an orphaned account that has not been noticed yet.

## Measured on

Provisioning time · over-privileged accounts created at provisioning (target zero) · temporary seats without an expiry (target zero)
