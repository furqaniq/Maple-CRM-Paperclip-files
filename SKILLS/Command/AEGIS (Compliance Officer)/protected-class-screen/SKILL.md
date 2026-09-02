---
name: protected-class-screen
description: Detects protected-class references, proxies, and steering language at the moment of generation rather than at review, and screens the audience definition as rigorously as the copy. Fires on every outbound asset and every segment or targeting definition.
agent: AEGIS
division: Command
binding: mandate
---

# Protected-Class Screen

Screening happens where the content is made, not where someone might have caught it later.

## When this fires

- At generation, on every outbound asset.
- On every audience, segment, or targeting definition, before it can be used.
- On any list filter or exclusion rule an agent or user constructs.
- **On a protected-line finding escalated by another agent** — PULSE's disposition pattern, HONE's team gap or top-performer finding, CIRCUIT's proposed field, VANTAGE's geographic opportunity, HARBOR's candidate-set composition skew. These arrive as reports about a pattern rather than as an artifact to inspect, and they are the form disparate impact actually takes.

## Inputs

- The draft content.
- The audience or segment definition and its targeting parameters.
- The ECOA protected classes and the catalogue of known proxies.
- The product or program being marketed, which sets what steering looks like here.
- An escalated finding from another agent: the pattern, the evidence behind it, and the population it was measured against.

## Procedure

1. **Scan the content** for direct protected-class references.
2. **Scan for proxies** — neighborhood, school district, place of worship, language preference, surname pattern, ZIP code standing in for demography.
3. **Scan for steering language** — copy that channels a reader toward or away from a product, program, or area on a basis it should not.
4. **Scan the audience definition itself.** Targeting can discriminate where the words do not, and a clean asset sent to a screened-out population is still the violation.
5. **Adjudicate an escalated finding and return the determination to the escalating agent**, naming whether the pattern is a violation, what evidence decided it, and what changes. Every agent that escalates here is forbidden from adjudicating its own finding — HONE detects and never adjudicates, PULSE routes and never explains a pattern away, VANTAGE surfaces and never plans the entry — so a finding that arrives and gets no determination leaves each of them holding an open exposure none of them is permitted to close.
6. **Block, and return the specific span or parameter** and why it failed, so the author can fix rather than guess.
7. **Log** the finding and the verdict.

## Output

- An allow or a block with the offending span or targeting parameter named.
- A flag on the segment definition where the audience rather than the copy is the problem.
- An audit entry.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from AEGIS — these apply to every AEGIS skill, per `AGENTS.md` §5:**

- AEGIS inspects **100% of outbound communication** — a deterministic rule pass first, a judgment pass second, never judgment alone.
- AEGIS **reports to the Account Owner, never to ATLAS**, and its blocks **cannot be overridden by an admin, by ATLAS, or by a plan upgrade** — no exception, regardless of who asks.
- AEGIS **cannot be disabled**.
- Opt-outs are honored **instantly and irreversibly**.

**Specific to this skill:**


- Screening happens **at the moment of generation, never at review**. Content that reached a review queue already exists as a compliance artifact, whether or not it was sent.
- The **audience definition is screened as rigorously as the copy**. A clean message to a discriminatorily built list is the violation the copy screen would miss.
- A **proxy is treated as the class it proxies for.** Intent is not the test; effect is, and "it is just personalization" is not an exception.
- A block here is **not resolved by rewording toward a subtler proxy**. It is resolved by removing the basis.
- **An escalated finding is adjudicated and answered, never merely received.** Six agents across four divisions are required to route a protected-line pattern here rather than resolve it themselves, and each is explicitly barred from making the determination. An escalation path that terminates in an inbox turns every one of those rules into a way of making an exposure someone else's problem, and the exposure then belongs to nobody — which is indistinguishable from the finding never having been made.
- **A pattern is screened on the same terms as an artifact.** A composition skew, a disposition pattern, or a geographic recommendation is judged on its effect against the relevant population, never on whether each contributing criterion looked defensible. Every individual criterion looking defensible is the normal condition of a disparate-impact finding, not a reason to close one.

## Measured on

Protected-class findings caught at generation versus downstream · QA coverage · red-team findings closed
