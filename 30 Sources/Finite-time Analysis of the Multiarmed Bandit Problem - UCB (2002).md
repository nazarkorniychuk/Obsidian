---
type: source
source-type: paper
authors: [Peter Auer, Nicolò Cesa-Bianchi, Paul Fischer]
year: 2002
url: https://consensus.app/papers/details/71cc7d6328ef5d319e9211205d30694b/?utm_source=claude_desktop
doi: 10.1023/a:1013689704352
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-27
aliases: [Auer 2002, UCB1 paper]
---

# Finite-time Analysis of the Multiarmed Bandit Problem - UCB (2002)

(~7,600 citations — the paper that made "optimism in the face of uncertainty" practical.)

## Summary

Lai & Robbins (1985) proved regret must grow at least **logarithmically** in the number of plays and gave asymptotically optimal but complex policies. This paper achieves logarithmic regret **uniformly over time** with simple, efficient index policies, for any bounded reward distribution.

## Key results

- **UCB1**: play the arm maximizing $\bar{x}_j + \sqrt{\tfrac{2\ln n}{n_j}}$; expected regret $\le \Big[8\sum_{i:\mu_i<\mu^*}\tfrac{\ln n}{\Delta_i}\Big] + O(1)$ where $\Delta_i = \mu^* - \mu_i$
- **UCB1-Tuned** (variance-aware) dominates in practice; **UCB2** tightens constants
- **ε-greedy with decaying schedule** $\epsilon_t = \min\{1, cK/(d^2 t)\}$ also achieves logarithmic regret — but requires knowing gap parameter $d$; fixed ε gives linear regret

## Impact

The exploration bonus $\sqrt{\ln n / n_j}$ became the template: MCTS's UCT rule ([[Monte Carlo Tree Search]]), count-based exploration bonuses in deep RL, and the theory behind "visit the uncertain thing" everywhere.

## Concepts extracted

- [[Exploration vs Exploitation]] — the quantitative core
