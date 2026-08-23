---
type: source
source-type: paper
authors: [Gabriel Smithline, et al.]
year: 2026
url: https://consensus.app/papers/details/3ec6a22be4725c6cb5b97ac4610f40eb/?utm_source=claude_desktop
doi: 10.48550/arxiv.2605.09403
topics: [deep-learning]
status: processed
rating: 2
created: 2026-08-23
aliases: [Smithline 2026]
---

# Sparsity Moves Computation (2026)

*Full title: "Sparsity Moves Computation: How FFN Architecture Reshapes Attention in Small Transformers."*

## Summary

Mechanistic study (one-layer transformers on arithmetic/counting tasks) of how the FFN's architecture changes what the *rest* of the model learns. Compares dense FFN, GLU, MoE, MoE-GLU.

## Key results

- **Sparse MoE routing shifts computation from FFN into attention** (strongest on carry-based addition) — FFN design choices have *non-local* consequences
- **Frozen random routing ≈ learned routing**: the redistribution comes from architectural sparsity itself, largely not from router-learned expert specialization
- GLU-style gating rotates task structure out of the per-neuron basis into distributed subspaces → **neuron-level interpretability degrades** while computation stays structured

## Why it's in the vault

Two deflationary results worth remembering: MoE routers may specialize less than assumed, and GLU quietly breaks neuron-level interpretability (tension with the key-value memory reading of [[Transformer Feed-Forward Layers Are Key-Value Memories (2021)]]).

## Caveat

Toy scale (one-layer, synthetic tasks); directional evidence, not LLM-scale fact.

## Concepts extracted

- [[Feedforward Network]], [[Mixture of Experts]]
