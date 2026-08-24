---
type: source
source-type: paper
authors: [Jakub Krajewski, Jan Ludziejewski, et al.]
year: 2024
url: https://consensus.app/papers/details/b9b8e11da40e528f83c8f9093f5031c6/?utm_source=claude_desktop
doi: 10.48550/arxiv.2402.07871
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-24
aliases: [Krajewski 2024, granularity scaling laws]
---

# Scaling Laws for Fine-Grained Mixture of Experts (2024)

## Summary

Introduces **granularity** $G$ as an explicit hyperparameter — how many pieces the FFN is sliced into (higher $G$ = more, smaller experts at fixed active compute) — and fits scaling laws over tokens, model size, and $G$.

## Key results

- **MoE consistently outperforms dense transformers**, and — contradicting [[Unified Scaling Laws for Routed Language Models (2022)|Clark 2022]] — the **efficiency gap *widens* with scale** once granularity is optimized (Clark's pessimism came from fixing expert size = FFN size)
- **The common practice of making each expert the size of the dense FFN is suboptimal at almost every compute budget** — finer is better, up to a point
- Provides compute-optimal $(N, D, G)$ configurations for a given budget

## Impact

The empirical justification for the fine-grained turn (DeepSeekMoE, Qwen-MoE): many small experts, not few big ones. Theoretical counterpart: exponential expressivity separation in granularity ([[The Power of Fine-Grained Experts (2025)]]).

## Concepts extracted

- [[Mixture of Experts]] — granularity scaling law
