---
type: source
source-type: paper
authors: [John Schulman, Philipp Moritz, Sergey Levine, Michael Jordan, Pieter Abbeel]
year: 2015
url: https://consensus.app/papers/details/eda61cc4848d505186368269bd0e1816/?utm_source=claude_desktop
doi: 10.48550/arxiv.1506.02438
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [GAE paper, Schulman 2015 GAE]
---

# High-Dimensional Continuous Control Using GAE (2015)

(~4,800 citations.)

## Summary

Names the two chronic problems of policy gradients — sample hunger and instability under nonstationary incoming data — and attacks the first with **Generalized Advantage Estimation**: an exponentially-weighted average of all n-step advantage estimators, **explicitly analogous to TD(λ)**, with two knobs γ (horizon) and λ (bias–variance) trading variance reduction against bootstrap bias.

## Key results

- $\hat{A}^{GAE(\gamma,\lambda)}_t = \sum_{l\ge0} (\gamma\lambda)^l \delta_{t+l}$ where $\delta_t$ is the TD error — λ=0 gives the one-step TD advantage (low variance, biased), λ=1 the Monte Carlo advantage (unbiased, high variance)
- With a trust-region method for **both** policy and value function: learned running gaits for 3D bipeds and quadrupeds, and standing up from the ground — neural policies straight from raw kinematics to joint torques
- Effective settings γ ∈ [0.99, 0.995], λ ∈ [0.92, 0.98]; the amount of simulated experience ≈ **1–2 weeks of real time** — strong for the era, and an honest record of on-policy sample cost

## Impact

The default advantage estimator: the "GAE" in every PPO implementation, including RLHF pipelines.

## Concepts extracted

- [[Generalized Advantage Estimation]] — the estimator
- [[Temporal Difference Learning]] — the λ-dial, reused
