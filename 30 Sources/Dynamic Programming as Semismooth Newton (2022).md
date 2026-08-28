---
type: source
source-type: paper
authors: [Matilde Gargiani, et al.]
year: 2022
url: https://consensus.app/papers/details/0efe0e949bc2542ab5cbe85b7b37c5be/?utm_source=claude_desktop
doi: 10.1109/lcsys.2022.3181213
topics: [reinforcement-learning]
status: processed
rating: 3
created: 2026-08-27
aliases: [Gargiani 2022, policy iteration Newton]
---

# Dynamic Programming as Semismooth Newton (2022)

(~13 citations.)

## Summary

Places the two classical MDP solvers in optimization terms for finite MDPs: **policy iteration is exactly the (semismooth) Newton method** applied to the Bellman equation, and **value iteration is fixed-point iteration**.

## Key results

- Policy iteration therefore inherits **local quadratic convergence** — the theoretical explanation for the empirical fact that PI converges in remarkably few iterations even with huge policy spaces
- Value iteration is the slow-but-global linear-rate method; the paper builds an accelerated VI with global guarantees from this view

## Why it's in the vault

Completes the vault's "everything is an optimization algorithm in disguise" thread: PI : VI :: Newton : gradient descent.

## Concepts extracted

- [[Bellman Equation]] — the PI/VI convergence-rate asymmetry
