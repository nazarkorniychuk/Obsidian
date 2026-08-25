---
type: source
source-type: paper
authors: [Dmitry Lepikhin, et al. (Google)]
year: 2020
url: https://consensus.app/papers/details/6cda916116105ffb8efe734cc8a70c1b/?utm_source=claude_desktop
doi: 10.48550/arxiv.2006.16668
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Lepikhin 2020]
---

# GShard (2020)

*Full title: "GShard: Scaling Giant Models with Conditional Computation and Automatic Sharding"* (~2,300 citations).

## Summary

The missing link between [[Outrageously Large Neural Networks (2017)|Shazeer 2017]] (MoE between LSTM layers) and [[Switch Transformers (2021)|Switch]]: **first MoE inside the transformer FFN** at scale, plus the compiler/sharding machinery (XLA annotations) to train it. Introduced the recipe later work iterated on: **top-2 gating, capacity factor** (fixed per-expert token buffer → the token-dropping problem [[MegaBlocks (2022)]] later solved), random dispatch, auxiliary balancing loss.

## Key results

- **600B-parameter** multilingual MT model trained on **2048 TPU v3 in 4 days**; far superior 100-language→English translation vs prior art
- Established that MoE + automatic sharding scales practically, not just in principle

## Why it's in the vault

"GShard-style" is the baseline every later MoE paper measures against ([[DeepSeekMoE (2024)]] frames its whole contribution as fixing GShard's specialization failure).

## Concepts extracted

- [[Mixture of Experts]] — the transformer-MoE template
