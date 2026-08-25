---
type: source
source-type: paper
authors: [Jun Gao, Di He, Xu Tan, Tao Qin, Liwei Wang, Tie-Yan Liu]
year: 2019
url: https://consensus.app/papers/details/f06fc10a7be0564da7a7c9587df7e798/?utm_source=claude_desktop
doi: 10.48550/arxiv.1907.12009
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Gao 2019, anisotropy paper]
---

# Representation Degeneration Problem (2019)

*Full title: "Representation Degeneration Problem in Training Natural Language Generation Models"* (ICLR 2019, ~400 citations).

## Summary

Identifies **embedding anisotropy**: training generation models by likelihood maximization (especially with weight tying) drives most learned word embeddings into a **narrow cone** — high average pairwise cosine similarity — rather than spreading over the space, limiting representational power.

## Key results

- Analyzes the cause: the likelihood objective pushes *all* non-target embeddings in a shared negative-gradient direction (dominated by frequent-token updates), collapsing directions
- A cosine-regularization fix improves LM and MT over baselines

## Why it's in the vault

The geometric pathology of the [[Embedding]]/[[Unembedding]] space: "similarity" measured in a degenerate cone is inflated, which matters for every embedding-based retrieval/analysis use; also connects to later logit-divergence instabilities and their fixes (z-loss family — see [[ST-MoE (2022)]]).

## Concepts extracted

- [[Embedding]], [[Unembedding]] — geometry section
