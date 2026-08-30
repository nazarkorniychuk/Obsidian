---
type: source
source-type: paper
authors: [Scott Fujimoto, Herke van Hoof, David Meger]
year: 2018
url: https://consensus.app/papers/details/6f2e36266f8a536999cb57d299138c6b/?utm_source=claude_desktop
doi: 10.48550/arxiv.1802.09477
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [TD3, Fujimoto 2018]
---

# Addressing Function Approximation Error - TD3 (2018)

(~7,300 citations.)

## Summary

Shows that [[Q-Learning|Q-learning's overestimation bias]] **persists in continuous-control actor-critic** (DDPG-style) settings and harms policies there too. **TD3** = DDPG + three fixes: **twin critics** taking the *minimum* of two Q-estimates in the target (the [[Double Q-learning (2010)|Double-Q]] decoupling idea, hardened into a pessimistic min), **delayed policy updates** (update the actor less often than the critics), and target-policy smoothing.

## Key results

- Outperformed the state of the art **in every environment tested** (OpenAI Gym continuous control)
- Established clipped-double-Q as the default bias control for off-policy continuous RL — inherited directly by SAC

## Concepts extracted

- [[Q-Learning]] — overestimation, third confirmation at scale
- [[Value-Based vs Policy-Based RL]] — the hybrid middle, hardened
