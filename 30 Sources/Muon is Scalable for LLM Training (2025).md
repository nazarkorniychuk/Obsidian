---
type: source
source-type: paper
authors: [Jingyuan Liu, et al. (Moonshot AI / Kimi)]
year: 2025
url: https://consensus.app/papers/details/441d1a4ab2db5dc8baea7ed52928d703/?utm_source=claude_desktop
doi: 10.48550/arxiv.2502.16982
topics: [deep-learning, optimization]
status: processed
rating: 4
created: 2026-08-25
aliases: [Muon paper, Moonlight, Liu 2025]
---

# Muon is Scalable for LLM Training (2025)

(~340 citations. Muon itself was introduced by Jordan et al. 2024 in the nanoGPT-speedrun community; this is the paper that proved it at LLM scale.)

## Summary

**Muon**: take the momentum matrix $M$ of each hidden weight matrix and **orthogonalize** it — replace $M$ by (approximately) $UV^\top$ from its SVD, computed cheaply via ~5 **Newton–Schulz iterations** — then step. Equivalent view: steepest descent under the **spectral norm**, flattening the update's singular values to 1 so all directions of the matrix update move at equal strength (vs Adam's element-wise view that ignores matrix structure). Applied to 2D hidden matrices only; embeddings/gains stay on AdamW.

## Key results

- Two techniques unlock scale: **add weight decay** and set **per-parameter update scale** consistently — then Muon works without hyperparameter tuning
- Scaling-law experiments: **~2× computational efficiency vs AdamW** at compute-optimal training
- **Moonlight**: 3B/16B-param MoE trained on **5.7T tokens** with Muon — improved the Pareto frontier; open-source memory-optimal distributed implementation (Muon carries only momentum state ≈ half of AdamW's)

## Follow-ups & caveats

NorMuon adds neuron-wise second moments (+11% over Muon at 1.1B); fair-benchmark work finds Muon-class gains **shrink with model/data scale under standard weight decay** ([[Fantastic Pretraining Optimizers II - Hyperball (2026)]]) — the honest current picture: real but scale-dependent speedup, with the update-geometry idea (spectral/orthogonal) now the main direction of optimizer research.

## Concepts extracted

- [[Optimizer]] — the post-Adam frontier
