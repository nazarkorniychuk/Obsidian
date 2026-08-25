---
type: source
source-type: paper
authors: [Tri Dao, Daniel Y. Fu, Stefano Ermon, Atri Rudra, Christopher Ré]
year: 2022
url: https://consensus.app/papers/details/b91073fdbce75d78a4cb24ff692323ec/?utm_source=claude_desktop
doi: 10.52202/068431-1189
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-24
aliases: [Dao 2022, FA1]
---

# FlashAttention (2022)

*Full title: "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness"* (~5,000 citations).

## Summary

The thesis: attention is slow not because of FLOPs but because of **HBM traffic** — the naive implementation materializes the $T \times T$ score matrix in slow GPU memory. FlashAttention is **IO-aware exact attention**: tile Q/K/V into blocks that fit in on-chip SRAM, compute softmax **online** (running max + running sum, rescaling as blocks arrive), never write the $T\times T$ matrix. Same math, exact result, $O(T)$ extra memory instead of $O(T^2)$; provably fewer HBM accesses (optimal for a range of SRAM sizes).

## Key results

- **15%** end-to-end speedup over the MLPerf 1.1 BERT-large record; **3×** on GPT-2 (seq 1K); **2.4×** on Long-Range Arena
- Longer context becomes affordable → *better models*: 0.7 better GPT-2 perplexity, +6.4 points long-document classification; **first transformer above chance on Path-X (16K)** — 61.4%
- Block-sparse FlashAttention: faster than every approximate-attention method it was compared to

## Impact

Universal adoption (PyTorch SDPA, every LLM stack). Strategic consequence: by making *exact* attention fast, it **erased the speed advantage that approximate/sparse attention methods were selling** — a key reason the efficient-attention zoo failed to displace dense attention.

## Concepts extracted

- [[Attention Mechanism]] — hardware section
