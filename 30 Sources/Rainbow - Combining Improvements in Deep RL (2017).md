---
type: source
source-type: paper
authors: [Matteo Hessel, Joseph Modayil, Hado van Hasselt, Tom Schaul, et al.]
year: 2017
url: https://consensus.app/papers/details/d27f5ee97d5d575996a5399a0e8d4880/?utm_source=claude_desktop
doi: 10.1609/aaai.v32i1.11796
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Rainbow, Hessel 2017]
---

# Rainbow - Combining Improvements in Deep RL (2017)

(~2,700 citations.)

## Summary

The integration study: takes **six independent DQN improvements** — [[Deep RL with Double Q-Learning - DDQN (2015)|Double]], [[Prioritized Experience Replay (2015)|prioritized replay]], [[Dueling Network Architectures (2015)|dueling]], **multi-step targets** (n-step returns, the TD(λ) idea), [[A Distributional Perspective on RL - C51 (2017)|distributional C51]], and **NoisyNets** (learned parametric exploration noise in the weights, replacing ε-greedy) — and tests whether they're complementary.

## Key results

- They are: the combination sets a new **state of the art on Atari in both data efficiency and final performance**
- The **ablation study** (remove one component at a time) is the paper's lasting value: **prioritized replay and multi-step targets were the most critical** — removing either hurt most; distributional learning mattered increasingly late in training; removing double Q-learning hurt least *within the full combination* (the distributional clamp already limits overestimation)

## Impact

The standard value-based baseline for years, and the field's clearest demonstration that independent fixes to different failure modes (bias, replay efficiency, architecture, target richness, exploration) **stack**.

## Concepts extracted

- [[Deep Q-Network]] — the culmination of the lineage + which parts matter
