---
name: approved-library-objection-handler
description: Answers objections from the compliance-approved library rather than improvising, because improvised objection handling is how compliance problems enter a system at scale. Fires on every objection intent.
agent: ECHO
division: Revenue
binding: mandate
---

# Approved-Library Objection Handler

An improvised objection answer is a compliance decision made at conversation speed, by the component least able to make it, thousands of times a day.

## When this fires

- On every message classified as an objection.
- On a question the approved library covers directly.
- On a library refresh, to re-check live conversations against retired or invalidated entries.

## Inputs

- The objection as the contact stated it, and the conversation so far.
- The compliance-approved library, as QUILL's `template-library-curator` maintains it and AEGIS has cleared it.
- The contact's record and brief, for the specifics an entry needs to be filled in with.
- The match confidence between the objection and the closest library entries.

## Procedure

1. **Match the objection to a library entry**, on what the contact actually raised rather than the nearest commercially convenient category.
2. **Answer from the entry where the match is confident**, personalizing only the record-derived specifics the entry is built to take.
3. **Escalate to a human where no entry matches.** This step has no branch and no improvised fallback.
4. **Escalate rather than combining entries.** Two approved answers spliced together is a third answer nobody approved.
5. **Route the composed reply through AEGIS's `two-pass-gate`** like any other outbound.
6. **Report the unmatched objection to QUILL's `template-library-curator`**, so a recurring gap becomes a new approved entry rather than a recurring escalation.
7. **Record which entry answered which objection**, so entry-level performance and entry-level compliance are both measurable.

## Output

- A reply drawn from a single approved entry, personalized only in its record-derived slots.
- A human escalation where nothing matched, with the objection quoted.
- A library-gap report naming the unmatched objection.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from ECHO — these apply to every ECHO skill, per `AGENTS.md` §5:**

- ECHO **never improvises objection handling** — only from the compliance-approved library.
- ECHO **exits permanently** on opt-out, hostility, legal language, or wrong number — no further contact, no exceptions.
- Complaints and distress are escalated to a human **within the same minute**, never batched.

**Specific to this skill:**

- **Objection handling is never improvised — this is ECHO's own boundary and it has no exceptions.** Not for a near-miss match, not under time pressure, not for an objection that seems trivial, not because the improvised sentence would obviously be fine. The rule is absolute because the judgment about whether an improvisation is fine is exactly the judgment that cannot be made reliably at this volume.
- **No entry is combined with another.** Splicing two approved answers produces an unapproved one while every component looks cleared, and the seam is where the compliance problem sits.
- **Personalization fills the slots an entry defines and changes nothing else.** Rewriting an entry to sound more natural is improvisation with a template as an alibi.
- **A retired or invalidated entry is never used**, including in a conversation already in progress. QUILL retires entries for compliance reasons and the conversation mid-flight is exactly where a retired one would still be sitting.
- **No entry is used to state a rate, payment, term, or cost figure without AEGIS's `disclosure-builder`**, and none is used to state an eligibility outcome at all.
- **An unmatched objection escalates and is reported.** Escalation alone leaves the same gap open tomorrow; the report is what closes it.
- **A human escalation carries the objection as the contact worded it**, not as the classifier categorized it. The wording is usually the point.

## Measured on

Objections answered from the library · improvised answers sent (target zero) · unmatched objections reported and converted to approved entries · escalation precision
