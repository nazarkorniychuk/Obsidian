---
type: source
source-type: paper
authors: [Joan Puigcerver, Carlos Riquelme, Basil Mustafa, Neil Houlsby]
year: 2023
url: https://consensus.app/papers/details/390e7cd6dbeb567da6ec1effd7564ae1/?utm_source=claude_desktop
doi: 10.48550/arxiv.2308.00951
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Soft MoE, Puigcerver 2023]
---

# From Sparse to Soft Mixtures of Experts (2023)

## Summary

The fully-differentiable escape from discrete routing: instead of assigning tokens to experts by top-k (non-differentiable, unstable, drops tokens), **Soft MoE** gives each expert a fixed number of *slots*, and each slot processes a **learned weighted mixture of all input tokens**. No discrete selection anywhere → ordinary end-to-end gradients, no balancing losses, no token dropping, no routing instability.

## Key results

- In vision: greatly outperforms dense ViTs and the popular sparse MoEs (Tokens-Choice, Experts-Choice) at matched cost
- Scales absurdly well: Soft MoE Huge/14 with 128 experts has **>40× more parameters than ViT-Huge at only +2% inference time**

## Caveat

The token-mixing trick fits bidirectional vision encoders naturally; adapting it to causal/autoregressive LMs is non-trivial (mixing future tokens breaks causality) — which is why LLMs still use sparse routing.

## Concepts extracted

- [[Mixture of Experts]] — the differentiable-routing alternative
