---
type: source
source-type: paper
authors: [Enric Boix-Adserà, et al.]
year: 2025
url: https://consensus.app/papers/details/d683fb328e9b510a988ea127f799dc9f/?utm_source=claude_desktop
doi: 10.48550/arxiv.2505.06839
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Boix-Adserà 2025]
---

# The Power of Fine-Grained Experts (2025)

*Full title: "The power of fine-grained experts: Granularity boosts expressivity in Mixture of Experts."*

## Summary

Theory companion to the empirical granularity results: compares architectures with many active experts per layer (DeepSeek-style, 8) vs few (Llama-4-style, 1).

## Key results

- **Proves an exponential separation in expressivity** based on granularity: at matched active parameters, many-small-experts networks can represent functions that few-big-experts networks need exponentially more capacity to match
- Experiments corroborate the separation

## Why it's in the vault

The intuition: $\binom{mN}{mK}$ grows combinatorially — finer slicing buys exponentially more distinct expert *mixtures* per token at the same compute. Backs [[Scaling Laws for Fine-Grained Mixture of Experts (2024)]] and [[DeepSeekMoE (2024)]] with a theorem.

## Concepts extracted

- [[Mixture of Experts]] — why granularity matters
