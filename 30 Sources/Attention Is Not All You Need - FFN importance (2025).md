---
type: source
source-type: paper
authors: [Isaac Gerber]
year: 2025
url: https://consensus.app/papers/details/88cca7ff4fc959c9a4bf41de763b373f/?utm_source=claude_desktop
doi: 10.48550/arxiv.2505.06633
topics: [deep-learning]
status: processed
rating: 2
created: 2026-08-23
aliases: [Gerber 2025]
---

# Attention Is Not All You Need - FFN importance (2025)

*Full title: "Attention Is Not All You Need: The Importance of Feedforward Networks in Transformer Models."*

## Summary

Ablation study on decoder-only pre-training: how much does the FFN matter, and is the standard 2-layer shape optimal?

## Key results

- Removing/shrinking FFNs measurably degrades pre-training loss — FFN capacity is load-bearing, not decorative
- **3-layer FFNs with fewer transformer blocks beat the standard 2-layer configuration**: lower training loss with fewer total parameters in less wall-clock time
- Suggests the 2-layer FFN convention is habit, not optimum — consistent with the hourglass findings of [[Revisiting the Shape Convention of Transformer Language Models (2026)]]

## Caveat

Small-scale study (7 citations); the 3-layer claim lacks LLM-scale replication.

## Concepts extracted

- [[Feedforward Network]] — shape/depth section
