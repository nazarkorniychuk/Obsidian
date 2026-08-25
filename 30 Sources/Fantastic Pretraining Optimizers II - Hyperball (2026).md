---
type: source
source-type: paper
authors: [Kaiyue Wen, et al.]
year: 2026
url: https://consensus.app/papers/details/5f7f6f288f6257109a7922eea7e68978/?utm_source=claude_desktop
doi: 10.48550/arxiv.2606.16899
topics: [deep-learning, optimization]
status: processed
rating: 3
created: 2026-08-25
aliases: [Wen 2026, Hyperball]
---

# Fantastic Pretraining Optimizers II - Hyperball (2026)

*Full title: "Fantastic Pretraining Optimizers and Where to Find Them II: Hyperball Optimization."* (Sequel to the fair-benchmark study of pretraining optimizers.)

## Summary

The referee result for the optimizer wars: matrix-based optimizers (Muon-class) genuinely speed up pretraining, **but the gains over AdamW shrink as model size and data scale grow** when using standard constant decoupled weight decay. Diagnosis via the weight-norm equilibrium: weight decay sets an equilibrium norm that determines the *angular* learning rate. Fix — **Hyperball**: wrap any base optimizer, fixing the Frobenius norms of weight matrices and updates to constants.

## Key results

- Muon-Hyperball: **20–30% token-equivalent speedup** over weight-decay baselines on Qwen3-style models up to 1.2B
- Improves learning-rate transfer across widths and depths vs decoupled weight decay (a μP-adjacent benefit)

## Why it's in the vault

Two lessons: (1) headline optimizer speedups must be read against scale (small-scale wins often deflate), and (2) weight decay is not just regularization — it's the **angular-velocity controller** of training, interacting with everything.

## Concepts extracted

- [[Optimizers]] — the benchmark-honesty caveat
