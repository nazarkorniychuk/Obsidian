---
type: source
source-type: paper
authors: [Aran Komatsuzaki, et al. (Google)]
year: 2022
url: https://consensus.app/papers/details/e52e9f4d56f05909bbb552f90a3eb14a/?utm_source=claude_desktop
doi: 10.48550/arxiv.2212.05055
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Komatsuzaki 2022, upcycling paper]
---

# Sparse Upcycling (2022)

*Full title: "Sparse Upcycling: Training Mixture-of-Experts from Dense Checkpoints."*

## Summary

Initialize a sparse MoE from a **trained dense checkpoint**: copy the dense FFN into every expert, add a fresh router, continue training. Reuses sunk pre-training cost instead of training MoE from scratch.

## Key results

- Upcycled T5 (Base/Large/XL) and ViT (Base/Large) **significantly outperform their dense parents** on SuperGLUE / ImageNet using only **~50% additional compute** on top of the original dense cost
- Also beats sparse-from-scratch when the from-scratch budget equals the dense sunk cost

## Impact

Standard industrial recipe for mid-scale MoEs (Nemotron-family, Qwen-MoE lineage, Mixtral is widely believed upcycled from Mistral-7B).

## Concepts extracted

- [[Mixture of Experts]] — upcycling variant
