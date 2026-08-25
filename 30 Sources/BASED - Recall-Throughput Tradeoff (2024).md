---
type: source
source-type: paper
authors: [Simran Arora, et al. (Stanford Hazy Research)]
year: 2024
url: https://consensus.app/papers/details/588019b9a8aa52bc929b4c7bff91c7cc/?utm_source=claude_desktop
doi: 10.48550/arxiv.2402.18668
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Arora 2024, BASED]
---

# BASED - Recall-Throughput Tradeoff (2024)

*Full title: "Simple linear attention language models balance the recall-throughput tradeoff."*

## Summary

Names the fundamental law of efficient attention: a **state-size ↔ recall tradeoff**. Attention's KV cache grows with context → perfect recall, heavy memory. Fixed-state models (Mamba, H3, RWKV, linear attention) have $O(1)$ memory → **provably and empirically struggle at recall** (grounding generations in earlier context). You cannot have both extremes at once — you choose a point on the frontier.

## Key results

- **BASED** = linear attention (global gist) + tiny sliding-window attention (precise local recall); varying window size and feature dimension **traverses the recall-memory Pareto frontier** end to end
- At 1.3B: matches Mamba in perplexity, **+6.22 accuracy points on real recall-intensive tasks**
- IO-aware kernels: **24× higher generation throughput than FlashAttention-2** (1024-token generations)

## Why it's in the vault

The theoretical frame that explains the entire efficient-attention landscape in one axis — and why every recent winner (BASED, [[Samba (2024)]], Griffin, production hybrids) is a *combination* of a compressed global state and exact local attention.

## Concepts extracted

- [[Linear Attention]] — the governing tradeoff; [[KV Cache]]
