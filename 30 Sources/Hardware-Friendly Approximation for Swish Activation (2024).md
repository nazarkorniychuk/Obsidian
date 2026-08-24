---
type: source
source-type: paper
authors: [Kangjoon Choi, et al.]
year: 2024
url: https://consensus.app/papers/details/b77ed27a4ee757a1abf620b12123d2e0/?utm_source=claude_desktop
doi: 10.1109/tcsii.2024.3394806
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Choi 2024]
---

# Hardware-Friendly Approximation for Swish Activation (2024)

## Summary

The hardware cost problem for smooth activations: Swish needs a sigmoid (exponential + division) per element — expensive in silicon. The known workaround **h-swish** (piecewise-linear approximation, used by MobileNetV3) reduced cost but left room. This paper proposes a better hardware approximation and its circuit implementation.

## Key results

- Beats h-swish on **delay, area, and power** simultaneously while preserving Swish's training-performance advantage over ReLU
- Confirms the general pattern: smooth activations are deployed in hardware as **piecewise/polynomial approximations**, never exactly

## Concepts extracted

- [[Activation Function]] — hardware section
