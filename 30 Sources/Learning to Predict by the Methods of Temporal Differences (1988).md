---
type: source
source-type: paper
authors: [Richard S. Sutton]
year: 1988
url: https://consensus.app/papers/details/26b0422fa8a450a08424e4abf2468a98/?utm_source=claude_desktop
doi: 10.1023/a:1022633531479
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-27
aliases: [Sutton 1988, TD paper]
---

# Learning to Predict by the Methods of Temporal Differences (1988)

(~4,900 citations — the founding paper of TD learning.)

## Summary

Conventional prediction learning assigns credit from the difference between *prediction and final outcome*; TD methods assign credit from the difference between **temporally successive predictions**. Introduces the TD(λ) family, proves convergence and optimality for special cases (in expectation, tabular/linearly-independent features), and relates TD to supervised learning.

## Key results

- For absorbing Markov chains with linearly independent features, TD(0) converges in expectation to the **true conditional-expectation predictions** — while for finite data, TD can be *more accurate than the maximum-likelihood/supervised estimate* on future data (the random-walk experiment)
- TD requires **less memory and less peak computation** than outcome-based methods (updates are incremental, no need to store episodes)
- Precursors identified: Samuel's checker player (1959), Holland's bucket brigade, the Adaptive Heuristic Critic

## Impact

The algorithmic core of value learning: Q-learning, SARSA, actor-critic critics, and GAE are all TD instances. Also the dominant computational model of dopamine signaling in neuroscience (Schultz–Dayan–Montague, 1997).

## Concepts extracted

- [[Temporal Difference Learning]] — the algorithm and the bias-variance position
