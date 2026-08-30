---
type: source
source-type: paper
authors: [Levente Kocsis, Csaba Szepesvári]
year: 2006
url: https://consensus.app/papers/details/2a5fcf88375354baaf9a8a4ef6bb3635/?utm_source=claude_desktop
doi: 10.1007/11871842_29
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-29
aliases: [UCT, Kocsis 2006]
---

# Bandit Based Monte-Carlo Planning - UCT (2006)

(~4,000 citations.)

## Summary

**UCT** ("Upper Confidence bounds applied to Trees"): treat every node of a search tree as a [[Exploration vs Exploitation|multi-armed bandit]] and select children by the UCB1 rule — turning Monte-Carlo planning from uniform sampling into a search that provably concentrates simulation effort on the most promising branches.

## Key results

- Consistency proof (converges to the optimal action given enough simulations) and finite-sample error bounds for finite-horizon/discounted MDPs
- Significantly more efficient than alternative Monte-Carlo planning across several domains; immediately made computer Go programs (MoGo) master-level on small boards

## Impact

The selection rule inside essentially every MCTS since, including (in polynomial-bonus variant) AlphaGo's PUCT.

## Concepts extracted

- [[Monte Carlo Tree Search]] — the core algorithm
