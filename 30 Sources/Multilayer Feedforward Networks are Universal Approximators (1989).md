---
type: source
source-type: paper
authors: [Kurt Hornik, Maxwell Stinchcombe, Halbert White]
year: 1989
url: https://consensus.app/papers/details/efe5fb4007db5e7f8ab08eb8c451025d/?utm_source=claude_desktop
doi: 10.1016/0893-6080(89)90020-8
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Hornik 1989, universal approximation theorem]
---

# Multilayer Feedforward Networks are Universal Approximators (1989)

(~24,500 citations; contemporaneous with Cybenko's sigmoid version.)

## Summary

**The universal approximation theorem**: a feedforward network with **one hidden layer** and any squashing activation can approximate any Borel-measurable function between finite-dimensional spaces to arbitrary accuracy — given enough hidden units. Later sharpened: the activation just needs to be **non-polynomial** (Leshno 1993); Hornik 1991 extended to derivatives and $L^p$ criteria.

## What it does and doesn't say

- **Says:** the architecture class is expressive enough — failures are failures of *optimization or generalization*, never of representational capacity
- **Doesn't say:** how many units (can be exponential), whether gradient descent finds the approximation, or whether it generalizes. Depth changes the *economics*: deep networks represent some functions with exponentially fewer units than shallow ones ([[Understanding Deep Neural Networks with Rectified Linear Units (2016)|Arora 2016]] for ReLU) — the theorem justifies the enterprise, depth separations justify the architecture

## Concepts extracted

- [[Neural Network]] — the expressivity foundation
