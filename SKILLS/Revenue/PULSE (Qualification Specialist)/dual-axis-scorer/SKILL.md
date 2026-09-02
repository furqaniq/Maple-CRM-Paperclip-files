---
name: dual-axis-scorer
description: Computes intent and fit as two independent scores and reports them separately, because a blended number destroys a rep's ability to triage. Fires on every material change to the discovery set or the record.
agent: PULSE
division: Revenue
binding: mandate
---

# Dual-Axis Scorer

Ready-but-hard and easy-but-cold are opposite problems, and one blended number hides which one you have.

## When this fires

- On completion of a discovery pass.
- On any material change to the discovery set, the enriched record, or the file's circumstances.
- On the recalibration cycle, when LEDGER returns updated outcome weights.

## Inputs

- The discovery answers, with each field's answered/partial/unasked state.
- The enriched record from SCOUT — property characteristics, tenure, labeled position estimates, market context.
- The scoring model's current weights and the calibration from [`calibration-feedback-loop`](../calibration-feedback-loop/SKILL.md).
- The feature set as screened by AEGIS's `protected-class-screen`.

## Procedure

1. **Compute intent — how ready this contact is to act — from stated timeline, motivation, decision-maker position, and observed engagement.**
2. **Compute fit — how workable the file is — from file and property characteristics only.**
3. **Report the two scores separately, always**, with no composite, no ranking, and no single headline number derived from them.
4. **State the coverage behind each score** — which discovery fields were answered and which were not — so a confident-looking score built on three of eight fields is visibly that.
5. **Attach the top contributing factors to each score**, so a rep can act on the reason rather than the number.
6. **Withhold a score rather than compute one on insufficient coverage**, and say which fields would produce it.
7. **Emit the scores as internal fields**, never as customer-facing output.

## Output

- An intent score and a fit score, separate, each with its coverage and its top contributing factors.
- A withheld-score state naming the missing fields, where coverage was insufficient.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from PULSE — these apply to every PULSE skill, per `AGENTS.md` §5:**

- PULSE **never states or implies an approval, denial, pre-approval, or eligibility outcome** — to a customer, in any channel, at any confidence level.
- A disqualification is an **internal routing state only** — never communicated to a customer as a decision.
- Autonomy is **permanently capped at L2** — this cap cannot be raised by AEGIS promotion or any other mechanism.

**Specific to this skill:**

- **The two scores are never blended, averaged, or collapsed into one.** A composite is the entire failure this skill exists to prevent: it makes a hot-but-complex file and a cold-but-easy file indistinguishable, which is exactly the distinction a rep needs at triage.
- **Fit is computed on the file, never on the person.** Property characteristics, product, tenure, and complexity are fit inputs. Name, language, neighborhood, apparent ethnicity, age, household composition, and every proxy for them are not — and a fit model that quietly learns a neighborhood weight is a redlining model with a workability label on it.
- **No credit information, and no reconstruction of one, is an input to either score.** SCOUT's firewall closes retrieval; this closes the door where an inferred score would actually get used.
- **Neither score is ever stated, shown, or paraphrased to a customer.** A score read out loud becomes an eligibility statement in the customer's ear no matter how it was framed — see [`outcome-language-interlock`](../outcome-language-interlock/SKILL.md).
- **Coverage travels with the score.** A score computed from two answered fields and one computed from eight look identical as numbers and are not remotely the same claim.
- **Insufficient coverage produces a withheld score, not a low one.** A low score reads as a judgment about the contact; an absent score reads as what it is, which is a gap in what the company asked.
- **PULSE is capped at L2 here, permanently.** The scores route and prioritize; they never disqualify anyone by themselves, never trigger an outbound decision, and never rise to acting on their own conclusion however strong the correlation gets.

## Measured on

Score-to-close correlation, per axis · composite scores produced (target zero) · scores withheld on low coverage · protected proxies in the feature set (target zero)
