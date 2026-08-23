---
type: source
source-type: paper
authors: [Bing Xu, Naiyan Wang, Tianqi Chen, Mu Li]
year: 2015
url: https://consensus.app/papers/details/1e20b8cfa3c15b1883a28a7d4a322104/?utm_source=claude_desktop
doi: 10.48550/arxiv.1505.00853
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-23
aliases: [Xu 2015, Leaky ReLU evaluation]
---

# Empirical Evaluation of Rectified Activations in Convolutional Network (2015)

## Summary

Systematic head-to-head of the ReLU family in CNNs: plain **ReLU**, **Leaky ReLU** (small fixed negative slope), **PReLU** (learned slope, from He et al. 2015), and their newly proposed **RReLU** (slope randomized during training). Image classification on CIFAR-10/100.

## Key results

- **Any non-zero negative slope consistently beats plain ReLU** — the paper's headline finding
- This is *evidence against* the popular belief that ReLU's sparsity (exact zeros) is the source of its power
- On small datasets, deterministic (Leaky) and learned (PReLU) slopes overfit; the randomized RReLU generalizes best — 75.68% on CIFAR-100 without ensembling

## Impact / adoption

Leaky ReLU became the standard "safe ReLU" in GANs (DCGAN discriminators) and detection backbones (YOLO/Darknet). PReLU was part of the first ImageNet result surpassing human-level top-5 error (He et al. 2015).

## Concepts extracted

- [[Activation Function]] — repair-family section; the anti-sparsity argument

## My questions

- Does the sparsity-doesn't-matter conclusion survive at transformer scale?
