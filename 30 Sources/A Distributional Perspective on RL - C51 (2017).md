---
type: source
source-type: paper
authors: [Marc G. Bellemare, Will Dabney, Rémi Munos]
year: 2017
url: https://consensus.app/papers/details/460698eba60c5fa6870ebd9ef0973b05/?utm_source=claude_desktop
doi: 10.48550/arxiv.1707.06887
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [C51, distributional RL, Bellemare 2017]
---

# A Distributional Perspective on RL - C51 (2017)

(~1,900 citations.)

## Summary

Argues the object of value learning should be the **full distribution of the random return**, not just its expectation $Q(s,a)$. Derives a distributional Bellman equation, proves the distributional operator is a contraction for *policy evaluation* (in Wasserstein distance) but exposes an **instability in the control case** — then builds **C51**: represent each return distribution as a categorical distribution over **51 fixed atoms** spanning $[-10, 10]$, and learn by projecting the distributional Bellman target back onto the atoms.

## Key results

- **State-of-the-art on Atari** at publication — remarkable for keeping the same greedy-in-expectation policy and only enriching the learning target
- The richer target acts as an auxiliary signal: the network must explain *why* returns vary (risk, multimodality), which shapes better representations

## Follow-ups

Quantile-regression variants (QR-DQN, IQN) removed the fixed-support restriction and improved further; a *Nature* companion found signatures of distributional codes in dopamine neurons. Component of [[Rainbow - Combining Improvements in Deep RL (2017)|Rainbow]].

## Concepts extracted

- [[Deep Q-Network]] — the richer-target item of the lineage
