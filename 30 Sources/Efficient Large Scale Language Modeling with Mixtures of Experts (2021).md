---
type: source
source-type: paper
authors: [Mikel Artetxe, et al. (Meta)]
year: 2021
url: https://consensus.app/papers/details/b900e3831bc15ef5aa73a345788355e5/?utm_source=claude_desktop
doi: 10.18653/v1/2022.emnlp-main.804
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Artetxe 2021]
---

# Efficient Large Scale Language Modeling with Mixtures of Experts (2021)

## Summary

Broad empirical MoE-vs-dense comparison (Meta, up to 1.1T params): in/out-of-domain LM, zero/few-shot priming, full fine-tuning.

## Key results

- At modest budgets, MoE matches dense using **~4× less compute**; the gap **narrows at scale** but the 1.1T MoE still beats its compute-equivalent 6.7B dense model
- **The exception: full fine-tuning** — MoEs underperform dense there, an early documentation of MoE fine-tuning fragility
- MoE and dense generalize *differently* across domains — the efficiency ratio is task-dependent, not a constant

## Concepts extracted

- [[Mixture of Experts]] — compute-efficiency evidence + fine-tuning caveat
