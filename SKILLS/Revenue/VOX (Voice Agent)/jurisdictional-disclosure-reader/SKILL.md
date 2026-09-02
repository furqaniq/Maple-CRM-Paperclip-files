---
name: jurisdictional-disclosure-reader
description: Delivers the AI disclosure required by the contact's jurisdiction at call open — never paraphrased, never shortened, never buried after pleasantries. Fires at the open of every call, inbound and outbound.
agent: VOX
division: Revenue
binding: mandate
---

# Jurisdictional Disclosure Reader

The disclosure is the first thing said on the call, in the words the jurisdiction requires, or the call does not proceed.

## When this fires

- At the open of every call, inbound and outbound, before any substantive exchange.
- On a jurisdiction resolving differently than expected mid-call — a stated location that contradicts the number's area code.
- On a change to the disclosure set for any jurisdiction the account operates in.

## Inputs

- The resolved jurisdiction for this contact, and the basis on which it was resolved.
- The required disclosure text for that jurisdiction, from AEGIS's `disclosure-builder`.
- The sending user's licensing and identity, where the disclosure requires them.
- The localized approved version, where the call is not in the account's default language.

## Procedure

1. **Resolve the jurisdiction before the call connects**, from the number, the record, and the stated address.
2. **Apply the most protective applicable disclosure set** where the jurisdiction is ambiguous or two could apply.
3. **Deliver the disclosure verbatim at call open**, before any pleasantry, question, or substantive exchange.
4. **Deliver it in the contact's language from the approved localized set**, never machine-translated in the moment.
5. **Re-deliver where the jurisdiction resolves differently mid-call** rather than continuing under the wrong one.
6. **End the call rather than proceed** where no valid disclosure is available for the resolved jurisdiction.
7. **Record the disclosure delivered, its version, and its timestamp within the call**, to AEGIS's `audit-ledger`.

## Output

- The verbatim disclosure delivered at call open, in the contact's language.
- An audit record naming the disclosure, its version, and its position in the call.
- A terminated call where no valid disclosure existed for the resolved jurisdiction.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VOX — these apply to every VOX skill, per `AGENTS.md` §5:**

- Recording consent and the AI disclosure are **handled per jurisdiction**, delivered at call open, never paraphrased, shortened, or buried after pleasantries.
- A human transfer request is honored **immediately, always, with no exception** — no retention attempt, no request for a reason.
- Voicemail drops are **capped at one per contact per day across every agent**, not just VOX.

**Specific to this skill:**

- **Verbatim, at open, in full — this is VOX's own boundary and it has no exceptions.** Never paraphrased to sound natural, never shortened because the caller is impatient, never moved after a greeting, never delivered as a summary, never skipped on a returning caller who has heard it before.
- **An ambiguous jurisdiction resolves to the most protective disclosure set.** Guessing toward the lighter obligation is the guess that produces the violation, and area codes have not tracked location for twenty years.
- **No valid disclosure means no call.** Ending the call is the correct outcome and is never worse than proceeding without one.
- **A required disclosure is never machine-translated on the fly.** The approved localized version exists or the call is not placed in that language.
- **A mid-call jurisdiction correction triggers re-delivery.** A contact who says they have moved has just told VOX that everything said so far was under the wrong rules.
- **Disclosure compliance is measured at one hundred percent and any miss is an incident**, not a rate to improve. This is the metric the handbook fixes at 100% and it is the only one on VOX's list stated that way.
- **The disclosure is never framed as optional, routine, or something to get past.** Tone that signals it does not matter is a paraphrase delivered by other means.

## Measured on

Disclosure compliance (must be 100%) · disclosures paraphrased, shortened, or delivered after the open (target zero) · calls terminated for a missing disclosure · jurisdiction resolution accuracy
