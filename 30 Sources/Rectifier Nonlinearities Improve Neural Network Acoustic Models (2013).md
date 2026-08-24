---
type: source
source-type: paper
authors: [Andrew L. Maas, Awni Y. Hannun, Andrew Y. Ng]
year: 2013
url: https://consensus.app/papers/details/3e404583ae14505196767800669f55f0/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Maas 2013, Leaky ReLU paper]
---

# Rectifier Nonlinearities Improve Neural Network Acoustic Models (2013)

**The Leaky ReLU origin** (ICML WDLASL 2013, ~7,700 citations).

## Summary

Speech recognition study (300h Switchboard): deep rectifier networks vs sigmoid for acoustic modeling, introducing the **leaky** variant — small non-zero slope for $x<0$ — to keep gradients alive in the negative regime.

## Key results

- Rectifier nets: **2% absolute WER reduction** over sigmoid counterparts, without pre-training
- Leaky ReLU performed comparably to ReLU here (its clear advantages showed up later in GANs/detection — see [[Empirical Evaluation of Rectified Activations in Convolutional Network (2015)|Xu 2015]])

## Concepts extracted

- [[Activation Function]] — Leaky ReLU origin
