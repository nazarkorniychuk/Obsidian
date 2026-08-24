---
type: source
source-type: paper
authors: [Guoliang Zhao, et al.]
year: 2025
url: https://consensus.app/papers/details/3ec5cafe38885be2bb1da992b591b56f/?utm_source=claude_desktop
doi: 10.48550/arxiv.2509.23678
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Zhao 2025, joint MoE scaling law]
---

# Towards a Comprehensive Scaling Law of Mixture-of-Experts (2025)

## Summary

The most complete joint MoE scaling law to date: **446 controlled experiments** over five factors — data $D$, total params $N$, active params $N_a$, number of active experts $G$, and **shared-expert ratio $S$** — fitting marginal effects and their couplings.

## Key results

- **Optimal $G$ (active experts) and $S$ (shared ratio) are *independent* of architecture and data size** — they can be tuned once, at small scale, and transferred
- **Optimal activation ratio $N_a/N$ decreases as total size $N$ grows** — bigger models should be *sparser* (matches the industry drift: Mixtral 28% → DeepSeek-V3 5.5%)
- Provides both theoretically-optimal and efficiency-aware-optimal configurations for $G$, $S$, $N_a/N$

## Concepts extracted

- [[Mixture of Experts]] — the shared-ratio and activation-ratio laws
