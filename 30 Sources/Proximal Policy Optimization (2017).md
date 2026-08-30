---
type: source
source-type: paper
authors: [John Schulman, Filip Wolski, Prafulla Dhariwal, Alec Radford, Oleg Klimov]
year: 2017
url: https://consensus.app/papers/details/1483ecc2b0b55ac4a3b6f10abb3bdffa/?utm_source=claude_desktop
doi: 10.48550/arxiv.1707.06347
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [PPO paper, Schulman 2017]
---

# Proximal Policy Optimization (2017)

(~30,000 citations — likely the most-used RL algorithm in existence.)

## Summary

Replaces TRPO's constrained second-order update with a **clipped surrogate objective** optimized by plain SGD:

$$L^{CLIP}(\theta) = \mathbb{E}\Big[\min\big(r_t(\theta)\hat{A}_t,\; \text{clip}(r_t(\theta),\, 1{-}\epsilon,\, 1{+}\epsilon)\,\hat{A}_t\big)\Big], \qquad r_t(\theta) = \tfrac{\pi_\theta(a_t \mid s_t)}{\pi_{\text{old}}(a_t \mid s_t)}$$

The clip removes the incentive to move the probability ratio outside $[1{-}\epsilon, 1{+}\epsilon]$ (ε = 0.2), and the outer **min makes the bound pessimistic** — clipping only ever *removes* reward for moving too far, never protects a harmful move. This is what safely enables the paper's headline feature: **multiple epochs of minibatch SGD on the same batch of rollouts**, where vanilla policy gradient permits exactly one update per sample.

## Key results

- Outperforms other online policy-gradient methods on continuous control (MuJoCo) and Atari; overall the best balance of **sample complexity, simplicity, and wall-time**
- Full recipe: clipped policy loss + value-function loss + entropy bonus, on [[High-Dimensional Continuous Control Using GAE (2015)|GAE]] advantages, from parallel rollouts — first-order only, compatible with shared parameters

## Impact

The default RL algorithm of the following decade, and the algorithm of **RLHF**: InstructGPT-style LLM post-training is PPO with per-token ratios plus a KL-to-reference penalty. See [[Implementation Matters - PPO vs TRPO (2020)]] for how much of its edge lives in code-level details.

## Concepts extracted

- [[PPO]] — the algorithm
