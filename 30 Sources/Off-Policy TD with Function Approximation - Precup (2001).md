---
type: source
source-type: paper
authors: [Doina Precup, Richard S. Sutton, Sanjoy Dasgupta]
year: 2001
url: https://consensus.app/papers/details/8646373c5809551d90998b17cae4c3ee/?utm_source=claude_desktop
topics: [reinforcement-learning]
status: processed
rating: 3
created: 2026-08-27
aliases: [Precup 2001]
---

# Off-Policy TD with Function Approximation - Precup (2001)

(~440 citations.)

## Summary

First off-policy TD algorithm **stable with linear function approximation**, via importance-sampling corrections that make the expected off-policy update equal the on-policy TD(λ) update.

## Why it's in the vault

Its problem statement is the cleanest record of the divergence facts: **Baird's counterexample (1995)** — a 7-state MDP with linearly independent features where an exact solution exists, yet Q-learning's approximate values **diverge to infinity**. Establishes that off-policy distribution mismatch (not approximation per se) drives the instability: the same TD(λ) update is stable when states are weighted by the on-policy distribution and divergent under the greedy-policy weighting.

## Concepts extracted

- [[Temporal Difference Learning]] — the deadly-triad evidence base
