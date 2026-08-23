---
type: source
source-type: paper
authors: [Ge Luo, et al.]
year: 2025
url: https://consensus.app/papers/details/bb0f8d2bab9254159b215b36d638c908/?utm_source=claude_desktop
doi: 10.3390/math14010039
topics: [deep-learning]
status: processed
rating: 2
created: 2026-08-23
aliases: [Luo 2025]
---

# ReLU Neural Networks and Their Training (2025)

## Summary

A consolidation paper (Mathematics, 2025): proves the universal approximation property for ReLU networks and — the part this vault cares about — gives a **theoretical account of the dying-ReLU / neuron-death phenomenon**, validated empirically.

## Key claims

- Formalizes *when* a ReLU neuron dies: once its pre-activation is negative for the whole data distribution, its gradient is identically zero and no future update can revive it
- Neuron death depends heavily on initialization; empirical experiments confirm the mechanism

## Why it's in the vault

The clean citation for the **dying-ReLU problem** — the flaw that motivated the entire Leaky/ELU/GELU repair lineage.

## Concepts extracted

- [[Activation Function]] — dying ReLU
- Motivates a future [[Weight Initialization]] note
