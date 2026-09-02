---
name: calibration-feedback-loop
description: Feeds every outcome back to LEDGER so scoring calibrates against what actually closed rather than what someone guessed, and monitors the model for proxy drift as it learns. Fires on every terminal outcome and on the recalibration cycle.
agent: PULSE
division: Revenue
binding: mandate
---

# Calibration Feedback Loop

A scoring model that never sees what closed is a set of opinions that hardens with age.

## When this fires

- On every terminal outcome — closed-won, closed-lost, permanently disqualified, or gone dark past its window.
- On the recalibration cycle.
- When realized outcomes diverge materially from what the scores predicted.

## Inputs

- The scores at the time of scoring, with their coverage and contributing factors.
- The realized outcome and its date.
- The conditions the file was worked under — producer, territory, lead volume, vintage — from LEDGER's `vintage-cohort-analyzer`.
- The current feature set, and AEGIS's `protected-class-screen` result on it.

## Procedure

1. **Emit every terminal outcome to LEDGER**, including the outcomes that make the model look bad.
2. **Send the score as it stood at scoring time**, never as later revised, so the loop measures the prediction rather than the hindsight.
3. **Carry the conditions the file was worked under**, so a calibration does not attribute a producer's territory to the contact's intent.
4. **Screen the feature set through AEGIS on every recalibration**, before new weights are adopted rather than after.
5. **Test for proxy drift explicitly**: check whether the learned weights have begun to track a protected characteristic through a correlated feature, and hold the recalibration if they have.
6. **Adopt new weights only after both screens pass**, and record what changed and why.
7. **Report calibration quality honestly**, including where the model is not predictive.

## Output

- Outcome records to LEDGER, carrying score-at-time, coverage, and working conditions.
- A recalibration proposal with its screening and proxy-drift results attached.
- An honest statement of where the model does not predict.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from PULSE — these apply to every PULSE skill, per `AGENTS.md` §5:**

- PULSE **never states or implies an approval, denial, pre-approval, or eligibility outcome** — to a customer, in any channel, at any confidence level.
- A disqualification is an **internal routing state only** — never communicated to a customer as a decision.
- Autonomy is **permanently capped at L2** — this cap cannot be raised by AEGIS promotion or any other mechanism.

**Specific to this skill:**

- **Proxy drift is tested for on every recalibration, and a failed test holds the weights.** This is the specific way a lawful scoring model becomes an unlawful one: nobody adds a protected characteristic, the model learns a feature that correlates with one, and each individual weight looks defensible. The loop is where it enters, so the check belongs here rather than in an annual review.
- **Every outcome is fed back, including the unflattering ones.** A loop fed selectively calibrates toward the cases that already agreed with it, and its confidence rises while its accuracy does not.
- **The score fed back is the score as it stood**, never a revised one. Feeding back a corrected score teaches the model that it was right.
- **Conditions are carried with the outcome.** Without them the model learns that leads worked by the best producer had the highest intent.
- **A new weight set is screened before adoption, never after.** A model running on unscreened weights for a quarter has already made a quarter of decisions on them.
- **Calibration quality is reported honestly, including where the model does not predict.** LEDGER's own rule is that an unfavorable number is never softened, and a model reporting on itself is exactly where softening happens.
- **PULSE's L2 cap is not a function of calibration quality.** A model that predicts perfectly is still capped, because the cap is about the legal adjacency of the decision and not about the accuracy of the guess.

## Measured on

Score-to-close correlation · recalibrations held on proxy drift · outcomes fed back as a share of terminal outcomes · unscreened weight sets adopted (target zero)
