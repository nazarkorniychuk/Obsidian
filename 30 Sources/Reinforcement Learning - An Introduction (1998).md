---
type: source
source-type: book
authors: [Richard S. Sutton, Andrew G. Barto]
year: 1998
url: https://consensus.app/papers/details/799eabb3233950e8941f27a4bfad3c51/?utm_source=claude_desktop
doi: 10.1109/tnn.1998.712192
topics: [reinforcement-learning]
status: processed
rating: 5
created: 2026-08-27
aliases: [Sutton & Barto, Sutton and Barto, RL textbook]
---

# Reinforcement Learning - An Introduction (1998)

(~44,000 citations for the 1st edition + ~2,200 for the 2nd (2018) — the canonical RL textbook.)

## Summary

Defines the field's shared vocabulary: the agent–environment loop, reward hypothesis, [[Markov Decision Process|MDP]] formulation (Part I), the three solution families — dynamic programming, Monte Carlo, and [[Temporal Difference Learning|temporal-difference]] learning (Part II) — and their unification via eligibility traces, function approximation, and planning (Part III).

## Key contributions (as used in the vault)

- The **DP–MC–TD triangle**: DP needs a model and bootstraps; MC needs no model and doesn't bootstrap; TD needs no model *and* bootstraps — the 2×2 that organizes every value-based method
- Names and formalizes the **deadly triad** (2nd ed.): function approximation + bootstrapping + off-policy training together can diverge; any two are safe
- ε-greedy, optimistic initialization, and UCB as the standard exploration toolkit; eligibility-trace forward/backward views

## Impact

The reference every RL paper implicitly assumes. Its notation ($V^\pi$, $Q^\pi$, $G_t$, $\delta_t$, $\gamma$) *is* the field's notation.

## Concepts extracted

- [[Markov Decision Process]] · [[Bellman Equation]] · [[Temporal Difference Learning]] · [[Exploration vs Exploitation]]
