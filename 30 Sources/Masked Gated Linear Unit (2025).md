---
type: source
source-type: paper
authors: [Yukito Tajima, et al.]
year: 2025
url: https://consensus.app/papers/details/7a29f2b33a1c59b484bb5bb096aac6fa/?utm_source=claude_desktop
doi: 10.48550/arxiv.2506.23225
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [MGLU paper, Tajima 2025]
---

# Masked Gated Linear Unit (2025)

## Summary

Attacks the hidden cost of GLU-family FFNs: **two separate weight matrices for gate and value streams = 2× the memory reads** of a non-gated FFN — a real inference bottleneck since LLM decoding is memory-bandwidth-bound. MGLU learns binary element-wise masks over a **single shared matrix** that assign each element to gate or value duty.

## Key results

- FlashMGLU kernel: up to **19.7× speedup** over naive PyTorch MGLU; **47% more memory-efficient and 34% faster than standard GLUs** on an RTX 5090
- SwiMGLU (Swish variant) **matches or exceeds SwiGLU downstream accuracy** while keeping the memory advantage

## Why it's in the vault

Quantifies the *cost* side of the SwiGLU trade: the quality win of gating ([[GLU Variants Improve Transformer (2020)]]) is paid for in memory bandwidth at inference — and that cost is engineerable away.

## Concepts extracted

- [[Feedforward Network]] / [[GLU Variants]] — gating cost section
