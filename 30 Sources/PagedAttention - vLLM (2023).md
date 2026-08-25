---
type: source
source-type: paper
authors: [Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu, Joseph E. Gonzalez, Hao Zhang, Ion Stoica]
year: 2023
url: https://consensus.app/papers/details/b03df83de2535d95aec8eea20e23d40b/?utm_source=claude_desktop
doi: 10.1145/3600006.3613165
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [vLLM paper, Kwon 2023]
---

# PagedAttention - vLLM (2023)

*Full title: "Efficient Memory Management for Large Language Model Serving with PagedAttention"* (SOSP 2023, ~7,700 citations).

## Summary

The KV cache is per-request state that **grows and shrinks dynamically**; pre-allocating contiguous max-length buffers wastes most of it (fragmentation + reservation). PagedAttention imports OS **virtual-memory paging**: the KV cache is split into fixed-size blocks (e.g. 16 tokens), allocated on demand, addressed through a per-request block table — logically contiguous, physically scattered.

## Key results

- **Near-zero KV memory waste** (vs severe waste in prior systems)
- **Copy-on-write sharing** of KV blocks within and across requests (parallel sampling, beam search, shared prefixes)
- vLLM: **2–4× throughput at equal latency** over FasterTransformer/Orca; gains grow with longer sequences and bigger models

## Impact

Became the de-facto open-source serving standard (vLLM); the paging idea spread to every serving stack (TGI, TensorRT-LLM, SGLang). Later refinements (vAttention, vTensor) recover virtual-memory contiguity via CUDA VMM.

## Concepts extracted

- [[KV Cache]] — memory management section
