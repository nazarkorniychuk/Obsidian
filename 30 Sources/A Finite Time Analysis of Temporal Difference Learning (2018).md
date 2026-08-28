---
type: source
source-type: paper
authors: [Jalaj Bhandari, Daniel Russo, Raghav Singal]
year: 2018
url: https://consensus.app/papers/details/dec9839ba8905a20a4390f02989183b7/?utm_source=claude_desktop
doi: 10.1287/opre.2020.2024
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Bhandari 2018]
---

# A Finite Time Analysis of Temporal Difference Learning (2018)

(~410 citations.)

## Summary

First clean **finite-time** (non-asymptotic) convergence rates for TD with linear function approximation — the classical results ([[The Convergence of TD(lambda) for General lambda (1992)|Dayan]], [[An Analysis of Temporal-Difference Learning with Function Approximation (1997)|Tsitsiklis & Van Roy]]) were asymptotic only. Key insight: TD updates can be analyzed by **mirroring online gradient descent**, even though the TD update is *not* a gradient of any fixed objective.

## Key results

- With i.i.d. noise: $O(1/\sqrt{T})$ error with constant step size + averaging; $O(1/T)$ with decaying steps — matching SGD-style rates
- Extends to Markovian (single-trajectory) sampling via an information-theoretic argument controlling the bias from correlated updates, and to TD(λ) and Q-learning for optimal stopping

## Concepts extracted

- [[Temporal Difference Learning]] — modern quantitative rates
