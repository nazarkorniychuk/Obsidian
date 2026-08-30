---
type: source
source-type: paper
authors: [Michael Janner, Justin Fu, Marvin Zhang, Sergey Levine]
year: 2019
url: https://consensus.app/papers/details/798c98010967554aaaed53cf73e909ff/?utm_source=claude_desktop
doi: 10.48550/arxiv.1906.08253
topics: [reinforcement-learning]
status: processed
rating: 4
created: 2026-08-29
aliases: [MBPO, Janner 2019]
---

# When to Trust Your Model - MBPO (2019)

(~1,300 citations.)

## Summary

The disciplined answer to model-based RL's core hazard — **compounding model error**: each imagined step feeds the model's own (slightly wrong) prediction back as input, so error grows with rollout length until the policy is training on fiction. MBPO's fix: **many short model rollouts branched from real states** — never let imagination run far from data — with a monotonic-improvement analysis incorporating measured model generalization to justify exactly how much model usage is safe.

## Key results

- Surpasses the sample efficiency of prior model-based methods **and matches the asymptotic performance of the best model-free algorithms** (the combination that had eluded the field: model-based methods were fast but plateaued low)
- Scales to horizons where other model-based methods fail entirely

## Concepts extracted

- [[Model-Based RL]] — compounding error and the short-rollout discipline
