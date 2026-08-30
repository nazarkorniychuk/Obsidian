---
type: source
source-type: paper
authors: [Yuntao Bai, Andy Jones, Kamal Ndousse, et al. (Anthropic)]
year: 2022
url: https://consensus.app/papers/details/04a1ee55eaca594eb30f5c83ca17b9a5/?utm_source=claude_desktop
doi: 10.48550/arxiv.2204.05862
topics: [reinforcement-learning, deep-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [Anthropic HH, Bai 2022]
---

# Helpful and Harmless Assistant - Anthropic HH (2022)

(~4,300 citations.)

## Summary

RLHF at assistant scale, with the engineering made explicit: helpfulness/harmlessness preference modeling, **iterated online training** (reward models and policies retrained weekly on fresh human feedback — attacking distribution shift at its source), and robustness analyses.

## Key results

- **Alignment training improved performance on almost all NLP evaluations** — an "alignment tax" mostly failed to appear; alignment is compatible with capability
- The empirical **linear relation between RL reward and √KL(policy ‖ init)** — the distance traveled from the initial policy predicts the reward gained, giving KL its status as *the* budget/measuring stick of RLHF
- Preference-model accuracy *drops* on comparisons between two high-quality samples — the RM is least reliable exactly where optimization pressure concentrates ([[Reward Model|overoptimization]]'s mechanism, observed directly)

## Concepts extracted

- [[RLHF]] — iterated online training, the KL budget
- [[Reward Model]] — where RM accuracy fails
