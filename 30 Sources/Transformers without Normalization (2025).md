---
type: source
source-type: paper
authors: [Jiacheng Zhu, Xinlei Chen, Kaiming He, Yann LeCun, Zhuang Liu]
year: 2025
url: https://consensus.app/papers/details/6bdd395dd4df554e980b0d4bd38b1a8a/?utm_source=claude_desktop
doi: 10.1109/cvpr52734.2025.01388
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [DyT paper, Zhu 2025]
---

# Transformers without Normalization (2025)

(CVPR 2025; He + LeCun among authors.)

## Summary

Observation: trained LayerNorms empirically implement **tanh-like S-shaped input-output mappings**. So replace normalization entirely with **Dynamic Tanh**: $\text{DyT}(x) = \tanh(\alpha x)$ with a learned scalar $\alpha$ — element-wise, no statistics computed, no mean/variance reductions.

## Key results

- Drop-in for LN across vision and language, supervised and self-supervised, recognition and generation: **matches or exceeds normalized counterparts, mostly without hyperparameter retuning**

## Why it's in the vault

Challenges the "normalization is indispensable" doctrine: what LN contributes at convergence may mostly be a **saturating squash**, obtainable without per-token statistics (which cost reductions/sync in kernels). Sits alongside the sigmoid-attention finding that attention sinks are softmax artifacts — a small genre of "the normalizer was the problem" results.

## Concepts extracted

- [[Normalization]] — the do-we-even-need-it result
