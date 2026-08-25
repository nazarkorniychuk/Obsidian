---
type: source
source-type: paper
authors: [Songlin Yang, Bailin Wang, Yikang Shen, Rameswar Panda, Yoon Kim]
year: 2023
url: https://consensus.app/papers/details/62b2905d6260536683b950d0a4b8f489/?utm_source=claude_desktop
doi: 10.48550/arxiv.2312.06635
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [GLA paper, FlashLinearAttention, Yang 2023]
---

# Gated Linear Attention (2023)

*Full title: "Gated Linear Attention Transformers with Hardware-Efficient Training"* (~500 citations).

## Summary

Fixes linear attention's two problems at once. (1) **Quality:** add **data-dependent gates** to the recurrent state update (learned forgetting — the same selectivity idea as [[Mamba (2023)]], in linear-attention form). (2) **Speed:** prior linear-attention implementations weren't IO-aware and lost to optimized softmax attention in practice; **FlashLinearAttention** trades memory movement against parallelism.

## Key results

- FlashLinearAttention is **faster than FlashAttention-2 as a standalone layer even at short lengths (1K)**
- GLA-Transformer competitive with LLaMA-architecture transformers and with Mamba/RetNet at moderate scale; **higher training throughput than same-size Mamba**
- Standout: **length generalization — trained at 2K, generalizes past 20K** without significant perplexity degradation

## Why it's in the vault

The convergence point: gated linear attention, selective SSMs, and RetNet-style decays are one family (input-dependent forgetting on a recurrent state) — and the IO-aware-kernel lesson from [[FlashAttention (2022)]] applies to attention *alternatives* too.

## Concepts extracted

- [[Linear Attention]] — the gated revival
