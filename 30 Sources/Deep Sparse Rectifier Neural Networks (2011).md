---
type: source
source-type: paper
authors: [Xavier Glorot, Antoine Bordes, Yoshua Bengio]
year: 2011
url: https://consensus.app/papers/details/5424ba055e0e5a8fa39fa0ac85f7268b/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Glorot 2011]
---

# Deep Sparse Rectifier Neural Networks (2011)

The second ReLU origin pillar (AISTATS 2011, ~8,900 citations), alongside [[Rectified Linear Units Improve Restricted Boltzmann Machines (2010)|Nair & Hinton 2010]].

## Summary

Argues rectifying neurons are both **more biologically plausible** than sigmoid/tanh and **equal or better for training** deep supervised networks — despite the hard non-linearity and non-differentiability at 0.

## Key results

- Deep rectifier nets match or beat tanh nets on image and text benchmarks, **without unsupervised pre-training** — a big deal in 2011, when pre-training was thought necessary for depth
- Popularized the **sparsity argument** (≈50% of units exactly zero) — later challenged by [[Empirical Evaluation of Rectified Activations in Convolutional Network (2015)|Xu 2015]]

## Concepts extracted

- [[Activation Function]] — ReLU origin; source of the (later-refuted) sparsity hypothesis
