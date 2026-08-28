---
type: source
source-type: paper
authors: [Yuri Burda, Harrison Edwards, Amos Storkey, Oleg Klimov]
year: 2018
url: https://consensus.app/papers/details/2e7996649db9597f83a76b3815c194e1/?utm_source=claude_desktop
doi: 10.48550/arxiv.1810.12894
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [RND, random network distillation]
---

# Exploration by Random Network Distillation - RND (2018)

(~1,700 citations.)

## Summary

**RND**: intrinsic reward = error of a predictor network trained to match the output of a **fixed, randomly initialized target network** on the current observation. Novel states → high error (predictor hasn't trained there); familiar states → low error. Because the target is deterministic, the bonus is **immune to environmental stochasticity** (no noisy-TV trap) — the fix for [[Curiosity-Driven Exploration by Self-Supervised Prediction - ICM (2017)|ICM]]'s failure mode.

## Key results

- First agent to exceed **average human performance on Montezuma's Revenge** without demonstrations or state access; occasionally clears level 1
- Introduces dual value heads to combine non-episodic intrinsic + episodic extrinsic returns with different discounts
- Minimal overhead: one extra forward pass + one small regression loss

## Impact

The default exploration bonus in deep RL since 2018; also reused *in reverse* as an uncertainty estimator (anti-exploration penalties in offline RL).

## Concepts extracted

- [[Exploration vs Exploitation]] — novelty via distillation error
