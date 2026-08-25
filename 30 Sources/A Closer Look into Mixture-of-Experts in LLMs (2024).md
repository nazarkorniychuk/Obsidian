---
type: source
source-type: paper
authors: [Ka Man Lo, et al.]
year: 2024
url: https://consensus.app/papers/details/a974fe8b1fbc5d56807227e210d1e590/?utm_source=claude_desktop
doi: 10.48550/arxiv.2406.18219
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Lo 2024]
---

# A Closer Look into Mixture-of-Experts in LLMs (2024)

## Summary

Empirical anatomy of three open MoE LLMs (Mixtral-class): what routers actually learn and how experts differ.

## Key findings

- **The router tends to select experts with larger output norms** — selection correlates with output magnitude, not just semantic fit
- **Expert diversity increases with depth** — deeper layers have more differentiated experts — except the last layer, an outlier
- Neurons act like *fine-grained experts* — expert-like specialization exists below the expert level
- Practical suggestions derived for router design and expert allocation

## Why it's in the vault

The observational check on routing stories: what balancing losses and specialization claims *actually* produce inside trained models.

## Concepts extracted

- [[Mixture of Experts]] — router behavior
