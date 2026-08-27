---
type: source
source-type: paper
authors: [Wenfang Sun, et al.]
year: 2025
url: https://consensus.app/papers/details/9e5e63da03c154318bb5df24617c4592/?utm_source=claude_desktop
doi: 10.48550/arxiv.2502.05795
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Sun 2025, curse of depth]
---

# The Curse of Depth in LLMs (2025)

*Full title: "The Curse of Depth in Large Language Models."*

## Summary

Documents that **nearly half the layers of modern LLMs are less effective than expected** (Llama, Mistral, DeepSeek, Qwen families) — and identifies **Pre-LN as the cause**: under pre-LN, the residual stream's variance grows with depth, so each deep block's LayerNorm-divided contribution shrinks and its derivative **approaches the identity matrix** — deep layers barely transform anything.

## Key results

- Theory + empirics: pre-LN output variance grows exponentially with depth → deep blocks ≈ identity
- Fix — **LayerNorm Scaling (LNS)**: scale each LN's output by $1/\sqrt{\ell}$ (inverse square-root of depth index). Consistently outperforms prior normalization schemes on 130M–7B pre-training; gains carry into fine-tuning

## Why it's in the vault

The other shoe of the pre-LN story: [[On Layer Normalization in the Transformer Architecture (2020)|Xiong 2020]] showed pre-LN fixes *training stability*; this shows it quietly *wastes depth*. Placement of one normalization op determines which half of the model does the work.

## Concepts extracted

- [[Normalization]], [[Residual Connection]] — depth pathology
