---
type: source
source-type: paper
authors: [Aidan Clark, et al. (DeepMind)]
year: 2022
url: https://consensus.app/papers/details/77947b689fb15f5087debc94feb8a4bd/?utm_source=claude_desktop
doi: 10.48550/arxiv.2202.01169
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Clark 2022]
---

# Unified Scaling Laws for Routed Language Models (2022)

## Summary

First systematic scaling laws for routed (MoE) models, across five orders of magnitude of size, hundreds of experts, three routing techniques. Treats **parameter count and compute as two independent axes** and fits a joint power law.

## Key results

- Derives an **Effective Parameter Count**: a single axis along which routed and dense models scale at the same rate — an N-expert MoE behaves like a dense model of some intermediate effective size
- **Routing benefits diminish as base model grows**: the boost from adding experts shrinks with scale (their fits suggested convergence around ~900B dense-equivalent) — an early "MoE has a ceiling" result later revised by [[Scaling Laws for Fine-Grained Mixture of Experts (2024)]], which found the opposite with granularity tuned
- Quantitative comparison of routing techniques (hash, RL, top-k) — learned top-k wins

## Concepts extracted

- [[Mixture of Experts]] — scaling laws section
