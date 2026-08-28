---
type: source
source-type: paper
authors: [Yuri Burda, Harri Edwards, Deepak Pathak, Amos Storkey, Trevor Darrell, Alexei A. Efros]
year: 2018
url: https://consensus.app/papers/details/906adda64c8d51c4950402c550130677/?utm_source=claude_desktop
doi: 10.48550/arxiv.1808.04355
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Burda 2018 curiosity]
---

# Large-Scale Study of Curiosity-Driven Learning (2018)

(~780 citations.)

## Summary

First large-scale audit of **purely curiosity-driven** learning — no extrinsic reward at all — across **54 environments** including the Atari suite.

## Key results

- Pure curiosity achieves surprisingly good *game scores*: the intrinsic objective happens to **align with hand-designed rewards** in many games (games are designed so novelty ≈ progress)
- **Random (fixed) features are often sufficient** for computing the prediction error on standard benchmarks; learned (inverse-dynamics) features generalize better to novel levels
- Documents the failure mode: with **stochastic transitions** (noisy-TV setup), prediction-error curiosity gets permanently captured by irreducible noise

## Impact

The random-features finding directly seeded [[Exploration by Random Network Distillation - RND (2018)|RND]]; the stochasticity result is the standard caveat cited against prediction-error bonuses.

## Concepts extracted

- [[Exploration vs Exploitation]] — what curiosity actually optimizes
