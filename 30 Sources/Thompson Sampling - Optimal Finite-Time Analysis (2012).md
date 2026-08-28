---
type: source
source-type: paper
authors: [Emilie Kaufmann, Nathaniel Korda, Rémi Munos]
year: 2012
url: https://consensus.app/papers/details/f0aa6c09e0195941987a5d58a9ce3373/?utm_source=claude_desktop
doi: 10.1007/978-3-642-34106-9_18
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Kaufmann 2012, Thompson sampling analysis]
---

# Thompson Sampling - Optimal Finite-Time Analysis (2012)

(~620 citations.)

## Summary

**Thompson sampling** (1933): maintain a posterior over each arm's reward parameter, sample one draw per arm, play the argmax — exploration by posterior randomization instead of explicit bonuses. Whether it was *optimal* stayed open for **79 years**.

## Key result

First finite-time analysis proving Thompson sampling **matches the Lai–Robbins lower bound** for Bernoulli rewards — i.e., it is asymptotically optimal, with finite-time regret bounds and empirical performance competitive with or better than KL-UCB.

## Impact

Rehabilitated the oldest bandit algorithm as a first-class citizen; posterior/randomized exploration is now the standard alternative to optimism (UCB), and the conceptual basis for bootstrapped/ensemble exploration in deep RL.

## Concepts extracted

- [[Exploration vs Exploitation]] — the Bayesian branch
