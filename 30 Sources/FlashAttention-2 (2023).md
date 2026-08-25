---
type: source
source-type: paper
authors: [Tri Dao]
year: 2023
url: https://consensus.app/papers/details/9106c78f94ad5f6e8be46d8b6c7d445b/?utm_source=claude_desktop
doi: 10.48550/arxiv.2307.08691
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [FA2, Dao 2023]
---

# FlashAttention-2 (2023)

*Full title: "FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning"* (~3,100 citations).

## Summary

FA1 reached only **25–40% of peak FLOPs** — the bottleneck moved from HBM traffic to **GPU work partitioning** (low occupancy, redundant shared-memory traffic between thread blocks/warps). FA2: fewer non-matmul FLOPs, parallelization across the sequence dimension even within one head, warp-level work split.

## Key results

- **~2× over FlashAttention-1**; **50–73% of theoretical peak** on A100 — approaching plain GEMM efficiency
- End-to-end GPT training at **225 TFLOPs/s per A100 (72% model-FLOPs utilization)**

## Concepts extracted

- [[Attention Mechanism]] — hardware section
