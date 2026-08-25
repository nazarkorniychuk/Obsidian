---
type: source
source-type: paper
authors: [Jonathan Frankle, Michael Carbin]
year: 2018
url: https://consensus.app/papers/details/ea5a27b051195f6fa25cf541fc5c6a03/?utm_source=claude_desktop
doi: 10.48550/arxiv.1803.03635
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Frankle 2018, lottery tickets]
---

# The Lottery Ticket Hypothesis (2018)

*Full title: "The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks"* (ICLR 2019 best paper, ~4,400 citations).

## Summary

Pruning removes 90%+ of weights *after* training — but the resulting sparse architectures famously can't be trained *from scratch*. The twist: they can, **if you rewind to their original initialization**. The hypothesis: dense random networks contain sparse subnetworks ("winning tickets") whose **initial weights** make them trainable in isolation to full-network accuracy.

## Key results

- Winning tickets at **10–20% of network size** match or beat the full network on MNIST/CIFAR, often learning *faster*
- The ticket is the (structure, initialization) *pair* — reinitializing the same structure destroys it

## Why it's in the vault

The sharpest evidence that **over-parameterization is a search procedure**: big networks win partly by drawing many initialization lottery tickets. Rhymes with attention-head redundancy ([[Are Sixteen Heads Really Better than One (2019)|Michel 2019]]'s train-wide-prune-later) and with [[Weight Initialization]]'s central claim that init determines trainability.

## Concepts extracted

- [[Neural Network]] — over-parameterization; [[Weight Initialization]]
