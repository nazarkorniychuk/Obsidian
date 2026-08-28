---
type: source
source-type: paper
authors: [Deepak Pathak, Pulkit Agrawal, Alexei A. Efros, Trevor Darrell]
year: 2017
url: https://consensus.app/papers/details/cade960da24d558b99b1af9e9eb13294/?utm_source=claude_desktop
doi: 10.1109/cvprw.2017.70
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [ICM, Pathak 2017, intrinsic curiosity module]
---

# Curiosity-Driven Exploration by Self-Supervised Prediction - ICM (2017)

(~3,100 citations.)

## Summary

**Intrinsic Curiosity Module (ICM)**: intrinsic reward = the error of a forward model predicting the next state — but crucially in a **learned feature space trained by an inverse-dynamics task** (predict the action from $\phi(s_t), \phi(s_{t+1})$). The inverse-dynamics objective makes features encode only what the agent's actions can affect, ignoring uncontrollable distractors.

## Key results

- Works under **sparse and even zero extrinsic reward**: agents explore VizDoom and clear Super Mario Bros levels driven by curiosity alone
- Curiosity-pretrained policies generalize to unseen levels (exploration as pretraining)
- Pixel-space prediction fails where feature-space prediction succeeds — the feature choice, not the prediction idea, is the contribution

## Known limitation

Prediction-error curiosity is bait for **irreducible stochasticity** (the "noisy-TV problem"): a random distractor yields permanent prediction error, hence permanent reward — documented at scale in [[Large-Scale Study of Curiosity-Driven Learning (2018)]].

## Concepts extracted

- [[Exploration vs Exploitation]] — prediction-error intrinsic motivation
