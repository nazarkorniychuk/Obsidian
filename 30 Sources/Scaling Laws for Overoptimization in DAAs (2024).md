---
type: source
source-type: paper
authors: [Rafael Rafailov, Yaswanth Chittepu, Ryan Park, et al.]
year: 2024
url: https://consensus.app/papers/details/a3f323b4c2e45d34a104296be42684ff/?utm_source=claude_desktop
doi: 10.48550/arxiv.2406.02900
topics: [reinforcement-learning]
status: processed
rating: 3
created: 2026-08-29
aliases: [DAA overoptimization, Rafailov 2024]
---

# Scaling Laws for Overoptimization in DAAs (2024)

(~150 citations. From the DPO authors themselves.)

## Summary

Tests whether direct alignment algorithms (DPO-family), having *removed* the proxy reward model, also removed reward hacking. **They didn't.**

## Key results

- At matched KL budgets, DPO-style methods show **degradation patterns similar to classic RLHF's overoptimization** — true quality peaks and declines while the training objective keeps improving
- Deterioration often begins **before even one epoch** over the preference dataset
- Conclusion: overoptimization is a property of *optimizing an imperfect proxy of human preference under finite data*, not of the explicit-RM architecture — the implicit reward inside DPO Goodharts just as well

## Why it's in the vault

The honest-caveat companion to [[Direct Preference Optimization (2023)|DPO]]: simplification did not repeal Goodhart's law.

## Concepts extracted

- [[DPO]] — the limits
- [[Reward Model]] — overoptimization generalizes beyond explicit RMs
