---
type: source
source-type: paper
authors: [Logan Engstrom, Andrew Ilyas, Shibani Santurkar, et al.]
year: 2020
url: https://consensus.app/papers/details/faa628da677f5a8d9fe378c5b8f90976/?utm_source=claude_desktop
doi: 10.48550/arxiv.2005.12729
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Engstrom 2020, code-level optimizations]
---

# Implementation Matters - PPO vs TRPO (2020)

(~340 citations. The honest-attribution paper for policy gradients.)

## Summary

Dissects "**code-level optimizations**" — augmentations found only in PPO's reference implementation, not its paper: value-function clipping, reward scaling, orthogonal initialization, learning-rate annealing, observation normalization and clipping, and more. Then ablates them against the *algorithmic* difference (clipping vs trust region).

## Key results

- The code-level optimizations are **responsible for most of PPO's cumulative-reward advantage over TRPO** — swap them between algorithms and the ranking largely follows the tricks, not the objective
- They **fundamentally change how the method functions** (e.g., they, not the clip, do much of the work of keeping the policy close to its predecessor)

## Why it's in the vault

Same genre as [[Fantastic Pretraining Optimizers II - Hyperball (2026)|Hyperball]] and [[On Bonus Based Exploration Methods - Taiga (2020)|Taiga]]: attribution honesty. "PPO beats TRPO" is really "PPO-the-artifact beats TRPO-the-artifact"; the named idea explains less than the engineering around it. Companion large-scale study: 50+ design choices, 250k trained agents, with concrete defaults ([[What Matters in On-Policy RL (2020)]]).

## Concepts extracted

- [[PPO]] — the implementation-details section
