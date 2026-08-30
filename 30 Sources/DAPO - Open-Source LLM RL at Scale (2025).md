---
type: source
source-type: paper
authors: [Qiying Yu, et al. (ByteDance Seed / Tsinghua)]
year: 2025
url: https://consensus.app/papers/details/a8e3d629b0675a30a3c2cc3d1ebf7dd6/?utm_source=claude_desktop
doi: 10.48550/arxiv.2503.14476
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [DAPO, Yu 2025]
---

# DAPO - Open-Source LLM RL at Scale (2025)

(~2,400 citations.)

## Summary

The reproducibility answer to closed reasoning-RL reports: a fully open large-scale system (code + data) reaching **50 points on AIME 2024 from a Qwen2.5-32B base**, built on GRPO with four fixes that turned out to be load-bearing:

- **Clip-Higher** — decouple the clip range upward (raise $1{+}\epsilon_{high}$) so low-probability tokens can grow, directly fighting **entropy collapse**
- **Dynamic sampling** — drop prompt groups whose rollouts are all-correct or all-wrong (zero advantage → zero gradient; the [[GRPO]] blind spot)
- **Token-level policy-gradient loss** — average over tokens, not per-sample, removing a length bias
- **Overlong reward shaping** — soft-penalize truncated responses instead of hard-failing them

## Why it's in the vault

The [[Implementation Matters - PPO vs TRPO (2020)|implementation-matters]] lesson, replayed in the RLVR era: the published algorithm is a fraction of the working system, and the fixes target exactly the failure modes ([[Exploration vs Exploitation|entropy collapse]], degenerate groups) the notes flag.

## Concepts extracted

- [[GRPO]] — the practical failure modes and fixes
- [[RLVR]] — open replication at scale
