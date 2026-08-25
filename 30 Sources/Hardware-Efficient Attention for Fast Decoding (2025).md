---
type: source
source-type: paper
authors: [Ted Zadouri, Hubert Strauss, Tri Dao]
year: 2025
url: https://consensus.app/papers/details/a8e27414bb8550ae914035f19ebebb87/?utm_source=claude_desktop
doi: 10.48550/arxiv.2505.21487
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Zadouri 2025, GTA GLA-attention]
---

# Hardware-Efficient Attention for Fast Decoding (2025)

## Summary

Redesigns attention around **arithmetic intensity** (compute per byte loaded): decode is bottlenecked by KV-cache loads, so do *more math per loaded byte*. Two designs: **GTA (Grouped-Tied Attention)** — tie K and V into one shared state per group; **GLA (Grouped Latent Attention)** — a parallel-friendly, shardable variant of [[Multi-Head Latent Attention|MLA]].

## Key results

- **GTA matches GQA quality with ~half the KV cache**
- **GLA matches MLA quality while being easier to shard** across GPUs (MLA's shared latent must be replicated under tensor parallelism; GLA groups it)
- Optimized GLA kernel **up to 2× faster than FlashMLA** (e.g., speculative-decoding regimes); up to 2× online-serving throughput

## Why it's in the vault

The current frontier of the KV lineage: after heads (GQA), layers (CLA), and rank (MLA), the fourth axis is **arithmetic intensity and shardability** — co-designing the attention variant with the parallelism scheme.

## Concepts extracted

- [[Grouped-Query Attention]], [[Multi-Head Latent Attention]], [[KV Cache]]
