---
type: source
source-type: paper
authors: [Runzhe Wang, Sadhika Malladi, Tianhao Wang, Kaifeng Lyu, Zhiyuan Li]
year: 2023
url: https://consensus.app/papers/details/b7e49aacc5b751b6a0b87713097d7916/?utm_source=claude_desktop
doi: 10.48550/arxiv.2307.15196
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Wang 2023, momentum skeptic paper]
---

# The Marginal Value of Momentum (2023)

*Full title: "The Marginal Value of Momentum for Small Learning Rate SGD."*

## Summary

The contrarian check on momentum folklore. Momentum provably accelerates *deterministic* strongly-convex gradient descent; the folklore extends this to stochastic training ("momentum reduces gradient-noise variance"). Theory here: **in the small-learning-rate, noise-dominated regime, SGD with and without momentum behave the same** over both short and long horizons — no provable acceleration.

## Key results

- Experiments confirm: momentum offers **limited optimization and generalization benefit** in practical regimes where the optimal LR isn't large — ImageNet from-scratch at small/medium batch, LM fine-tuning

## Why it's in the vault

The boundary condition on [[On the Importance of Initialization and Momentum (2013)|Sutskever 2013]]: momentum's value concentrates where **curvature** is the enemy (large stable LRs, large batches — i.e., LLM pre-training), not where **noise** is. Explains why $\beta_1$ matters more at frontier scale than in small fine-tunes.

## Concepts extracted

- [[Gradient Descent]] — momentum's honest scope
