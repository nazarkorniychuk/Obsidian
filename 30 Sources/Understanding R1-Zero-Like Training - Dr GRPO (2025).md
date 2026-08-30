---
type: source
source-type: paper
authors: [Zichen Liu, Changyu Chen, et al. (Sea AI Lab)]
year: 2025
url: https://consensus.app/papers/details/51430fe7a4b0506b825cdd2f68f73f16/?utm_source=claude_desktop
doi: 10.48550/arxiv.2503.20783
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Dr. GRPO, Liu 2025]
---

# Understanding R1-Zero-Like Training - Dr GRPO (2025)

(~1,250 citations.)

## Summary

The critical audit of the R1-Zero recipe, dissecting both components:

- **Base models:** DeepSeek-V3-Base *already exhibits "aha-moment"-style behavior before any RL*, and Qwen2.5 bases show strong reasoning without templates — part of what RL "makes emerge" is pretraining bias being surfaced
- **The algorithm:** GRPO's normalization carries an **optimization bias that artificially inflates response length** — especially of *incorrect* outputs (the per-response length division and std-normalization interact) — so "longer chains of thought" partly measures a training artifact, not deeper thought

## Key results

- **Dr. GRPO** (bias-removed variant): better token efficiency at equal reasoning performance
- A minimalist recipe reaching **43.3% AIME 2024 with a 7B base** — SOTA at that scale at publication

## Why it's in the vault

Two honest-caveat lessons in one: emergent-behavior claims need base-model controls, and objective-function details silently shape the observable "phenomena."

## Concepts extracted

- [[GRPO]] — the length bias
- [[RLVR]] — what "emergence" claims must control for
