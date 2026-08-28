---
type: source
source-type: book
authors: [Dimitri P. Bertsekas]
year: 1995
url: https://consensus.app/papers/details/65df1c8973c05614b543666eba65398a/?utm_source=claude_desktop
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Bertsekas DP, Bertsekas 1995]
---

# Dynamic Programming and Optimal Control - Bertsekas (1995)

(~8,100 citations, two volumes; continuously revised.)

## Summary

The control-theory-side textbook on dynamic programming: finite-horizon DP, infinite-horizon discounted/average-cost problems, and — crucially for RL — **approximate DP / neuro-dynamic programming**, where value functions are replaced by parametric approximators and exact DP breaks down.

## Key content (as used in the vault)

- The **curse of dimensionality**: exact DP cost scales with $|S|$, which grows exponentially in the number of state variables — the single reason function approximation (and hence all of deep RL) exists
- Error-bound machinery for approximate value iteration: how approximation error $\epsilon$ per step compounds into $O\!\big(\tfrac{\epsilon}{(1-\gamma)^2}\big)$ policy suboptimality
- Companion volume (Bertsekas & Tsitsiklis, *Neuro-Dynamic Programming*, 1996) is the bridge from DP theory to TD/Q-learning convergence analysis

## Concepts extracted

- [[Markov Decision Process]] — curse of dimensionality
- [[Bellman Equation]] — approximate DP error bounds
