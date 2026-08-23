---
type: source
source-type: paper
authors: [Abien Fred Agarap]
year: 2018
url: https://consensus.app/papers/details/b9cb4589c00257e38a1ad9dfb68688f5/?utm_source=claude_desktop
doi: 10.48550/arxiv.1803.08375
topics: [deep-learning]
status: processed
rating: 2
created: 2026-08-23
aliases: [Agarap 2018]
---

# Deep Learning using Rectified Linear Units (2018)

## Summary

Originally a small paper about using ReLU *as the classification layer* (in place of softmax). Its accidental importance: it became one of the **most mis-cited papers in deep learning** — thousands of works cite it as "the ReLU paper." A revised version formally corrects the record, tracing ReLU's lineage and crediting its definitive introduction into deep learning to **Nair & Hinton (2010)** (with earlier roots in Glorot et al.'s sparse-activation work and decades-old piecewise-linear biological models).

## Key results (revised version)

- Empirical re-comparison of ReLU vs tanh vs sigmoid across vision and text with statistical testing (Kruskal-Wallis, 10 trials)
- **Sigmoid fails to converge entirely** in deep convolutional tasks (chance-level accuracy) — the vanishing-gradient problem made concrete
- ReLU best on classification; tanh best on image reconstruction (bounded output helps there)

## Why it's in the vault

Two lessons: the correct citation for ReLU's origin (**Nair & Hinton 2010**), and hard evidence for *why* saturating activations died.

## Concepts extracted

- [[Activation Function]] — history/attribution + the sigmoid-collapse evidence
