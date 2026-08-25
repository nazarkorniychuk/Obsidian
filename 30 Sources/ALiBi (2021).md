---
type: source
source-type: paper
authors: [Ofir Press, Noah A. Smith, Mike Lewis]
year: 2021
url: https://consensus.app/papers/details/134f4e0c2807514489bf2b445df698bb/?utm_source=claude_desktop
doi: 10.48550/arxiv.2108.12409
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Press 2021, attention with linear biases]
---

# ALiBi (2021)

*Full title: "Train Short, Test Long: Attention with Linear Biases Enables Input Length Extrapolation"* (~1,300 citations).

## Summary

Drop positional embeddings entirely; instead **bias the attention logits by distance**: score$(i,j) = q_i \cdot k_j - m \cdot |i - j|$, with a fixed per-head slope $m$ (geometric sequence across heads — different heads decay at different rates). A built-in recency prior, no learned position parameters.

## Key results

- **1.3B model trained at length 1024 extrapolates to 2048 with the same perplexity** as a sinusoidal model *trained* at 2048 — while training 11% faster with 11% less memory
- Beat sinusoidal, rotary, and T5-relative baselines on WikiText-103

## The catch

Later receptive-field analysis showed ALiBi's "extrapolation" is largely an **implicit sliding window** — the linear penalty effectively stops it from using tokens beyond the training range (Chi 2022) — extrapolating perplexity but not truly *using* long context.

## Impact

Adopted by BLOOM and MPT; lost the standards war to [[RoFormer (2021)|RoPE]]+scaling tricks, but its bias-not-embedding idea recurs (relative biases, sink-aware designs).

## Concepts extracted

- [[Positional Encoding]] — ALiBi + the extrapolation caveat
