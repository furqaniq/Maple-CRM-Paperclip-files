---
name: capacity-aware-router
description: Routes each lead by territory, language, specialty, and real-time capacity — never into someone at capacity or off shift. Fires on every resolved record and again when an assignment goes unworked.
agent: SCOUT
division: Revenue
binding: mandate
---

# Capacity-Aware Router

A lead routed to someone who is off shift is a lead nobody answered, and speed to lead is the metric that decides the outcome.

## When this fires

- On every resolved record ready for assignment.
- When an assigned lead goes unworked past its first-touch window.
- When the org structure, a territory, or a person's availability changes with leads already assigned.

## Inputs

- The resolved record — territory, language, product, and complexity signals.
- Branch, team, territory, and routing rules from WARDEN's `org-structure-manager`.
- Live working hours and availability from TEMPO's `availability-engine`.
- Current open-lead load per person, and the account's configured capacity ceilings.

## Procedure

1. **Resolve territory, language, and specialty requirements from the record.**
2. **Read availability from TEMPO's `availability-engine`**, not from a routing-local shift table, so working hours have exactly one definition in the platform.
3. **Read structure and territory from WARDEN's `org-structure-manager`**, so a routing target is never a seat that no longer exists.
4. **Exclude anyone off shift or at capacity** before ranking the remainder — this is an exclusion, not a scoring penalty.
5. **Rank the eligible set** on fit and current load, and assign.
6. **Escalate rather than force-assign where the eligible set is empty**, naming which constraint emptied it, and route to the configured overflow so the lead is worked by someone rather than parked correctly.
7. **Re-route on an unworked assignment** within the first-touch window rather than waiting for the next cycle.

## Output

- An assignment naming the person, and the constraints that produced them.
- An overflow escalation where no eligible target existed, naming the emptying constraint.
- A re-route record where a first assignment went unworked.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from SCOUT — these apply to every SCOUT skill, per `AGENTS.md` §5:**

- SCOUT **never pulls, requests, infers, or stores consumer credit information**, under any circumstance.
- Position estimates are **always labeled as estimates** — never presented as fact.

**Specific to this skill:**

- **Capacity and shift are hard exclusions, not weights.** A weighted score will always eventually route into someone who is asleep, because every other factor was strong enough.
- **Availability comes from TEMPO and structure comes from WARDEN.** A router keeping its own shift calendar or its own territory map produces a second source of truth that disagrees with the first exactly when someone leaves or moves branch.
- **An empty eligible set escalates and overflows; it never silently queues.** A lead held until someone comes on shift is the same outcome as no routing at all, measured by the only metric that matters here.
- **Routing never uses language, name, or neighborhood as a proxy for anything but the stated operational requirement.** Matching a stated language preference is service; inferring one from a surname and steering on it is a fair-lending problem wearing a routing rule.
- **An unworked assignment is re-routed inside the first-touch window**, not reported at the end of the day when the lead has already gone cold.
- **Routing is never used to hide a lead from a person.** Excluding someone from a rotation is an org-structure decision belonging to WARDEN, made explicitly, not a silent routing weight.

## Measured on

Median time to first touch · leads routed to someone off shift or at capacity (target zero) · unworked assignments re-routed inside the window · overflow rate
