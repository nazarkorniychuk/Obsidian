---
type: source
source-type: paper
authors: [Noam Shazeer, Azalia Mirhoseini, Krzysztof Maziarz, Andy Davis, Quoc Le, Geoffrey Hinton, Jeff Dean]
year: 2017
url: https://consensus.app/papers/details/b52df32234275ea7a0a6170ce315212e/?utm_source=claude_desktop
doi: 10.48550/arxiv.1701.06538
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Shazeer 2017, sparsely-gated MoE paper]
---

# Outrageously Large Neural Networks (2017)

*Full title: "Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer"* (~5,400 citations).

## Summary

The modern [[Mixture of Experts]] origin: makes conditional computation actually work at scale. A trainable gating network selects a sparse combination from up to thousands of expert FFNs per example; introduces noisy top-k gating and load-balancing losses to prevent router collapse.

## Key results

- **>1000× model-capacity increase with only minor computational-efficiency loss** on GPU clusters
- **137B-parameter** MoE (between stacked LSTM layers — pre-transformer) — the largest neural network of its time, in 2017
- Beat SOTA on language modeling and machine translation **at lower computational cost**

## Impact

Established the core MoE recipe (sparse gating + top-k + auxiliary balancing losses) that [[Switch Transformers (2021)]] later simplified (top-1) and transplanted into the transformer FFN. Every modern MoE LLM (Mixtral, DeepSeek, Qwen-MoE) descends from this design. Also notable: Shazeer is an author of *both* this and [[GLU Variants Improve Transformer (2020)]] as well as [[Attention Is All You Need (2017)]] — one person behind the FFN's three biggest changes.

## Concepts extracted

- [[Mixture of Experts]] — origin
- [[Feedforward Network]] — MoE section
