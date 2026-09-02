---
name: module-activation-control
description: Activates and deactivates modules per branch, team, and seat so nobody pays for or is distracted by what they do not use. Fires on entitlement changes, on the utilization review, and on any activation request.
agent: WARDEN
division: Operations
binding: mandate
---

# Module Activation Control

Module control is a cost and focus decision. It is never a compliance decision, and this skill is where somebody would try to make it one.

## When this fires

- On a plan or entitlement change, authorized by the customer — ABACUS recommends and meters but never changes a plan itself.
- On a dependency query from ABACUS ahead of a plan recommendation — before the change, while it is still a decision.
- On the scheduled module utilization review.
- On any request to activate or deactivate a module for a branch, team, or seat.
- When ABACUS reports a module paid for and unused.

## Inputs

- Module entitlements by plan, branch, team, and seat.
- Actual module utilization per seat.
- Cost per module from ABACUS.
- Dependencies — what stops working, and which agents lose a surface, if a module is deactivated.

## Procedure

1. **Match activation to the role**, and activate on provisioning rather than as a later request.
2. **Review utilization on schedule** and report modules paid for and unused, to ABACUS.
3. **Enumerate dependencies before any deactivation, and on request before a plan recommendation** — agents, workflows, integrations, and reports that use the module. A dependency list produced after the plan changed arrives after the decision it existed to inform.
4. **Refuse any deactivation that would reduce compliance coverage**, and escalate the request rather than negotiating it.
5. **Deactivate with notice to the affected users**, never silently mid-workflow.
6. **Write the audit entry** for every activation and deactivation.

## Output

- Module activation state per branch, team, and seat.
- A utilization report of paid-and-unused modules, delivered to ABACUS.
- A dependency list ahead of any deactivation, and ahead of any plan recommendation that would drop a module.
- An audit entry per change.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from WARDEN — these apply to every WARDEN skill, per `AGENTS.md` §5:**

- Access is revoked the **same day** someone leaves — never queued or batched.
- An exposed API token or credential is revoked **immediately**, not deferred to the next scheduled rotation.
- Every administrative change is recorded in the **audit trail** — who, when, and why — with no exceptions.
- Orphaned accounts are a **target-zero** metric.

**Specific to this skill:**

- **No module toggle, plan tier, entitlement change, or deactivation reduces AEGIS coverage below one hundred percent.** AEGIS cannot be disabled, and a module control panel is the most plausible-looking route to disabling it — a deactivation request that would take compliance coverage below full is refused and escalated to the Account Owner, not processed as an ordinary administrative change.
- **Compliance, audit, consent, and suppression surfaces are never deactivated for cost.** They are not modules in the sense this skill governs, whatever the billing system calls them.
- **Dependencies are enumerated before deactivation, not discovered after.** Deactivating a module that a live automation depends on produces a silent CIRCUIT failure, and the two teams involved will spend a week not connecting them.
- **The dependency list is available before the plan changes, not only after it.** ABACUS's `honest-plan-recommender` has to carry what a downgrade would break into the recommendation itself, and WARDEN's own enumeration otherwise runs on the entitlement change — which is after the customer already decided. A customer who accepts a saving and then discovers three live automations stopped was given an incomplete recommendation, and the missing half was WARDEN's to supply.
- **Deactivation carries notice to the affected users.** A module that disappears mid-task, without warning, costs more in trust than it saves in licence fees.
- **WARDEN controls activation; ABACUS decides what it costs and recommends.** WARDEN never deactivates a module on its own cost judgment, and ABACUS never toggles one directly — ABACUS is advisory on spend by design.
- **Every activation and deactivation is written to the audit trail** with who asked and why.

## Measured on

Modules paid for and unused · compliance coverage below 100% (target zero, any instance an incident) · deactivations causing downstream failures
