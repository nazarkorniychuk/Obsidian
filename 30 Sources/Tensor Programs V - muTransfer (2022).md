---
type: source
source-type: paper
authors: [Greg Yang, Edward J. Hu, Igor Babuschkin, et al. (Microsoft/OpenAI)]
year: 2022
url: https://consensus.app/papers/details/ae47b70c70f25ee7bceb70d1d883cca6/?utm_source=claude_desktop
doi: 10.48550/arxiv.2203.03466
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [muP paper, muTransfer, Yang 2022]
---

# Tensor Programs V - muTransfer (2022)

*Full title: "Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer."*

## Summary

In standard parameterization, the optimal learning rate *shifts* as you widen a model — so hyperparameters tuned on a small model are wrong for the big one. **μP (Maximal Update Parametrization)** rescales initialization variances and per-layer learning rates with width such that **optimal hyperparameters become width-stable**. Then **μTransfer**: tune HPs on a tiny model, zero-shot transfer them to the giant one.

## Key results

- Transfer from a **13M**-param proxy outperformed published **BERT-large (350M)**; transfer from **40M** outperformed published **GPT-3 6.7B numbers at 7% of pre-training cost** in tuning
- μP is also the unique width-parameterization that preserves **feature learning** in the infinite-width limit (vs the "lazy" [[Neural Tangent Kernel - Jacot (2018)|NTK]] regime where features freeze)
- Extensions: Depth-μP, spectral-condition derivations for Muon/Shampoo-class optimizers, per-module transfer

## Impact

Standard practice at frontier labs (HP sweeps run at small scale and transferred); large-scale replications confirm near-optimal LR transfer to 10B/190B-token runs, with caveats in some settings.

## Concepts extracted

- [[Weight Initialization]] — the scaling-rules frontier; [[Neural Tangent Kernel]] — the lazy/feature-learning divide
