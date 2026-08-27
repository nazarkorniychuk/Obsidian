---
type: source
source-type: paper
authors: [Ruibin Xiong, et al.]
year: 2020
url: https://consensus.app/papers/details/f62b97cdae5852e9aad74f3d56ed99ab/?utm_source=claude_desktop
doi: 10.48550/arxiv.2002.04745
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Xiong 2020, pre-LN paper]
---

# On Layer Normalization in the Transformer Architecture (2020)

(ICML 2020, ~1,600 citations.)

## Summary

Why does transformer training require the mysterious learning-rate **warmup** stage? Answer: it's an artifact of **where LayerNorm sits**. Mean-field analysis at initialization:

- **Post-LN** (original Transformer: LN *after* the residual add): gradients near the output are **large** → big learning rates blow up → warmup exists to survive the early phase
- **Pre-LN** (LN *inside* the residual branch, before attention/FFN): gradients well-behaved at init → **warmup can be removed entirely**

## Key results

- Pre-LN without warmup matches baselines with less training time and tuning
- Explains the empirical folklore; formalized the design shift

## Impact

Pre-LN became the LLM default (GPT-2 onward, LLaMA-family with RMSNorm). Known costs discovered later: pre-LN's residual variance grows with depth so **deep layers approach identity mappings** ("curse of depth" — deep layers under-contribute), motivating LayerNorm-scaling, Peri-LN, and hybrid placements; [[DeepNet - Scaling Transformers to 1000 Layers (2022)]] took the other route, fixing Post-LN.

## Concepts extracted

- [[Normalization]] — the placement question
