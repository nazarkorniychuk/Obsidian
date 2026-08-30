---
type: source
source-type: paper
authors: [Yang Yue, Zhiqi Chen, Rui Lu, et al. (Tsinghua)]
year: 2025
url: https://consensus.app/papers/details/62a6a3fcafe45c2f8c6026573000ae2d/?utm_source=claude_desktop
doi: 10.48550/arxiv.2504.13837
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Yue 2025, pass@k limits]
---

# Does RL Incentivize Reasoning Beyond the Base Model (2025)

(~1,000 citations — the sharpest critique of the RLVR narrative.)

## Summary

Probes the *reasoning boundary* of RLVR-trained models with **pass@k at large k**: give the base model and the RL model many attempts each, and ask who can solve more problems *at all*.

## Key results

- RLVR models win at small k (pass@1) — but **at large k the base model matches or overtakes them**, across model families, RL algorithms, and math/code/visual benchmarks
- Coverage and perplexity analyses: the RL model's successful reasoning paths **already exist in the base model's distribution** — RLVR *sharpened sampling toward them* rather than creating them; six popular algorithms perform similarly and far from the base model's ceiling
- **Distillation, by contrast, genuinely expands the boundary** — a teacher can inject reasoning patterns the base lacks

## The debate it opened

Sharpening vs. discovery is now actively contested: CoT-Pass@k analyses argue RLVR does improve *reasoning-path* correctness; ProRL reports boundary expansion under prolonged training with KL resets; negative-sample analyses complicate the picture further. The honest current state: **at standard training scales, much of RLVR's gain is elicitation, not creation — whether and when it becomes creation is open.**

## Concepts extracted

- [[RLVR]] — the sharpening-vs-discovery caveat
