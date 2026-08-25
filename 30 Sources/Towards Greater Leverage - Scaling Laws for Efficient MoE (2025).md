---
type: source
source-type: paper
authors: [Changxin Tian, et al. (Ant Group / Ling team)]
year: 2025
url: https://consensus.app/papers/details/e1e8ad6f408a5795896c5c22089d8f00/?utm_source=claude_desktop
doi: 10.48550/arxiv.2507.17702
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Tian 2025, Efficiency Leverage]
---

# Towards Greater Leverage - Scaling Laws for Efficient MoE (2025)

## Summary

Defines **Efficiency Leverage (EL)** = the compute multiplier an MoE gets over a quality-matched dense model, and fits it over **300+ trained models up to 28B params**.

## Key results

- **EL is driven primarily by the activation ratio and total compute budget, both as power laws**; granularity acts as a **non-linear modulator with a clear optimal range** (not monotone — too fine hurts)
- Validation at scale: **Ling-mini-beta, 0.85B active params, matched a 6.1B dense model** trained on the same 1T tokens — **>7× less compute** for equal quality
- The unified law predicts EL of a configuration before training it

## Concepts extracted

- [[Mixture of Experts]] — the predictive activation-ratio law + the 7× validation
