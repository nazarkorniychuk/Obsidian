---
type: source
source-type: paper
authors: [Peter Dayan]
year: 1992
url: https://consensus.app/papers/details/2e43aca971d2508e8dd8fed64e1d2649/?utm_source=claude_desktop
doi: 10.1007/bf00992701
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-27
aliases: [Dayan 1992]
---

# The Convergence of TD(lambda) for General lambda (1992)

(~270 citations.)

## Summary

Extends Sutton's 1988 convergence theorem from TD(0) to **TD(λ) for arbitrary λ ∈ [0,1]** (in expectation), using Watkins' analysis machinery. Also examines TD under linearly *dependent* state representations: it still converges, but to a **different fixed point than least-mean-squares** — an early hint that TD's solution is not the projection of the true values.

## Follow-ups

- Dayan & Sejnowski (1994) strengthened this to **convergence with probability one** for TD(λ), with rate quantification
- The definitive function-approximation treatment came with [[An Analysis of Temporal-Difference Learning with Function Approximation (1997)|Tsitsiklis & Van Roy 1997]]

## Concepts extracted

- [[Temporal Difference Learning]] — convergence status of TD(λ)
