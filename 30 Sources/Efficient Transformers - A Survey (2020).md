---
type: source
source-type: paper
authors: [Yi Tay, Mostafa Dehghani, Dara Bahri, Donald Metzler]
year: 2020
url: https://consensus.app/papers/details/12aff8cf49165a73ad84e9e840b02889/?utm_source=claude_desktop
doi: 10.1145/3530811
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Tay survey, X-former survey]
---

# Efficient Transformers - A Survey (2020)

(ACM Computing Surveys, ~1,600 citations.)

## Summary

The taxonomy of the "X-former" zoo — Reformer, Linformer, Performer, Longformer, BigBird, Routing Transformer, Synthesizer, and dozens more — organized by mechanism: fixed patterns (local/strided), learnable patterns (clustering/LSH), low-rank projections, kernels, and memory tokens. Companion benchmark: **Long Range Arena** (LRA).

## Why it's in the vault

The historical record of a research program that mostly **didn't ship**: despite ~50 published sub-quadratic variants, every frontier LLM of the 2023–2026 era runs dense (Flash-accelerated) attention, at most hybridized with sliding windows. The autopsy, assembled across this vault's sources: quality gaps at scale ([[Simple Local Attentions Remain Competitive (2021)]]), LRA not predicting real-task performance (same paper), [[FlashAttention (2022)]] deleting the speed argument, and [[On The Computational Complexity of Self-Attention (2022)]] proving exact sub-quadratic is impossible anyway.

## Concepts extracted

- [[Attention Mechanism]] — the efficient-attention failure story
