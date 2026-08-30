---
type: source
source-type: paper
authors: [Marcin Andrychowicz, et al. (Google Brain)]
year: 2020
url: https://consensus.app/papers/details/abe2472cef655e6d9b02cdaedd567519/?utm_source=claude_desktop
doi: 10.48550/arxiv.2006.05990
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Andrychowicz 2020]
---

# What Matters in On-Policy RL (2020)

(~290 citations + ~250 for the conference version.)

## Summary

The brute-force answer to "which knobs actually matter": implements **50+ design choices** of on-policy actor-critic methods in one unified framework and trains **over 250,000 agents** across five continuous-control environments to measure each choice's impact.

## Key findings (the famous recommendations)

- **Policy initialization matters enormously**: initialize the last policy layer with ~100× smaller weights so the initial policy is near-uniform — one of the largest single effects found
- **Advantage normalization** (per-batch, mean-zero) and **observation normalization**: recommended defaults
- Use Adam (~3e-4, annealed); PPO-style clipping is the recommended loss; the *value-function clipping* trick, by contrast, showed little benefit
- Many published "innovations" matter less than these unglamorous choices — echoing [[Implementation Matters - PPO vs TRPO (2020)]]

## Why it's in the vault

The empirical bedrock under "implementation details that matter": when a PPO run fails, the fix is far more likely in this list than in the objective.

## Concepts extracted

- [[PPO]] — tuning defaults with evidence
- [[Actor-Critic]] — what actually moves the needle
