---
type: source
source-type: paper
authors: [Zixiang Chen, Yihe Deng, Yue Wu, Quanquan Gu, Yuanzhi Li]
year: 2022
url: https://consensus.app/papers/details/c8d29c3832ca5769941126666b510fd8/?utm_source=claude_desktop
doi: 10.52202/068431-1675
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Chen 2022, MoE collapse theory]
---

# Towards Understanding the Mixture-of-Experts Layer (2022)

## Summary

Theory (NeurIPS 2022) on the two standing puzzles: *why* does MoE help, and *why doesn't the mixture collapse* into one model?

## Key results

- **Cluster structure in the data + non-linearity of the experts are pivotal**: they prove a classification problem with intrinsic clusters that a single 2-layer CNN expert provably cannot solve, but an MoE of such experts can
- Mechanism: the **router learns cluster-center features**, dividing the complex problem into simpler per-cluster sub-problems that individual experts can conquer — specialization is *provably* the working mechanism (in this setting)
- *Linear* experts don't get this benefit — non-linearity is required for the division of labor to pay

## Why it's in the vault

Theoretical counterweight to [[Sparsity Moves Computation (2026)]]'s random-routing result: when data has genuine cluster structure, learned routing provably matters.

## Concepts extracted

- [[Mixture of Experts]] — why it works / doesn't collapse
