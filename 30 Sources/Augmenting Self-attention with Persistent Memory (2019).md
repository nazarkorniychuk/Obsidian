---
type: source
source-type: paper
authors: [Sainbayar Sukhbaatar, Edouard Grave, Guillaume Lample, Hervé Jégou, Armand Joulin]
year: 2019
url: https://consensus.app/papers/details/dc39a27b192152a883e135398af4ce94/?utm_source=claude_desktop
doi: 10.48550/arxiv.1907.01470
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [Sukhbaatar 2019, persistent memory paper]
---

# Augmenting Self-attention with Persistent Memory (2019)

## Summary

Shows the FFN is not architecturally sacred: adds trainable **persistent key-value vectors** to the attention layer (extra slots every token can attend to) and **removes the FFN entirely**.

## Key results

- All-attention transformer with persistent vectors **matches standard transformer** performance on character- and word-level language modeling benchmarks
- Constructive proof that the FFN is *functionally* an attention-like key-value lookup over stored (learned, input-independent) memories — two years before [[Transformer Feed-Forward Layers Are Key-Value Memories (2021)|Geva 2021]] showed the same by inspection

## Why it's in the vault

The FFN's job can be relocated, but not deleted — the parameters/memory capacity must live somewhere. Complements [[Attention Is Not All You Need - FFN importance (2025)]], which shows deleting FFN capacity outright hurts.

## Concepts extracted

- [[Feedforward Network]] — role section
- [[Attention Mechanism]]
