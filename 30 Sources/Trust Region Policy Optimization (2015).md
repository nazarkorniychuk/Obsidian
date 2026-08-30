---
type: source
source-type: paper
authors: [John Schulman, Sergey Levine, Philipp Moritz, Michael Jordan, Pieter Abbeel]
year: 2015
url: https://consensus.app/papers/details/e386f58084f25569bc251d1f0c70799e/?utm_source=claude_desktop
doi: 10.48550/arxiv.1502.05477
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [TRPO, Schulman 2015]
---

# Trust Region Policy Optimization (2015)

(~8,200 citations.)

## Summary

The theory-first answer to the policy-gradient step-size problem. Derives a lower bound on the true performance of a new policy in terms of (a) a surrogate objective computed on the *old* policy's data (importance-weighted advantages) and (b) a penalty on the KL divergence between old and new policies — so maximizing the bound gives **guaranteed monotonic improvement**. The practical algorithm approximates the penalty as a hard constraint:

$$\max_\theta\; \mathbb{E}\Big[\tfrac{\pi_\theta(a \mid s)}{\pi_{\text{old}}(a \mid s)}\, \hat{A}\Big] \quad \text{s.t.} \quad \overline{\text{KL}}(\pi_{\text{old}} \,\|\, \pi_\theta) \le \delta$$

solved with conjugate gradient + line search (second-order machinery).

## Key results

- Robust learning of simulated **swimming, hopping, walking gaits** and Atari from pixels, **with little hyperparameter tuning** — the reliability that made trust regions standard
- The monotonic-improvement analysis is the theoretical backbone later methods invoke

## Limitations that created PPO

Second-order optimization is complex and expensive; incompatible with parameter sharing (policy/value in one network) and architectures like dropout; hard to scale. [[Proximal Policy Optimization (2017)|PPO]] is the first-order approximation of exactly this scheme.

## Concepts extracted

- [[PPO]] — the lineage and the theory
- [[Policy Gradient]] — why step size is *the* problem
