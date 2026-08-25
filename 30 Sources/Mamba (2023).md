---
type: source
source-type: paper
authors: [Albert Gu, Tri Dao]
year: 2023
url: https://consensus.app/papers/details/05dd434bcc4b5cbf9b326abcc58d7aec/?utm_source=claude_desktop
doi: 10.48550/arxiv.2312.00752
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Gu & Dao 2023, selective SSM]
---

# Mamba (2023)

*Full title: "Mamba: Linear-Time Sequence Modeling with Selective State Spaces"* (~8,600 citations).

## Summary

Diagnosis of why prior sub-quadratic models (linear attention, S4-style SSMs) lost to attention on language: their state transitions are **input-independent** — they cannot *selectively* remember or forget based on content. Mamba makes the SSM parameters ($\Delta, B, C$) **functions of the input** (selectivity), sacrificing the convolutional training trick but replacing it with a **hardware-aware parallel scan** (kernel fusion, recomputation — a FlashAttention-style IO-aware design; Dao co-authored both).

## Key results

- **5× generation throughput** vs same-size transformers; linear scaling to million-length sequences
- **Mamba-3B outperforms same-size transformers and matches transformers 2× its size** on language modeling and downstream evals
- SOTA across language, audio, genomics at the scales tested

## Impact

Made "post-transformer" a serious research program; spawned Mamba-2/3, vision/time-series variants, MoE-Mamba — and, most consequentially in practice, **hybrids** ([[Samba (2024)]]-style SSM+attention). The recall weakness of fixed-size states remains its boundary ([[BASED - Recall-Throughput Tradeoff (2024)]]).

## Concepts extracted

- [[Linear Attention]] — the rebirth; future note [[State Space Models]]
