---
type: source
source-type: paper
authors: [David Rumelhart, Geoffrey Hinton, Ronald Williams]
year: 1986
url: https://consensus.app/papers/details/bfefb8f86c3e57b499501c128f87d76e/?utm_source=claude_desktop
doi: 10.1038/323533a0
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Rumelhart 1986, backprop paper]
---

# Learning Representations by Back-propagating Errors (1986)

**The backpropagation paper** (Nature, ~32,000 citations; with the book-chapter version ~21,000 more).

## Summary

The learning procedure for multi-layer networks: repeatedly adjust connection weights to minimize output error, computing each weight's gradient by **propagating error derivatives backward** through the network via the chain rule (the generalized delta rule).

## Key results

- **Hidden units learn useful internal representations** — features of the task domain not present in input or output — the capability that separates multi-layer learning from the perceptron
- Made training arbitrary differentiable architectures practical; every modern model, LLMs included, trains with (automated, batched, GPU-scaled) descendants of this exact procedure

## Context

The algorithm's math (reverse-mode automatic differentiation) predates the paper (Linnainmaa 1970; Werbos 1974 for NNs); Rumelhart-Hinton-Williams demonstrated its power for representation learning and made it the field's foundation. Biological plausibility remains debated — the brain likely does not implement weight-symmetric backprop, and random-feedback alternatives work surprisingly well (Lillicrap 2016).

## Concepts extracted

- [[Backpropagation]] — the origin
