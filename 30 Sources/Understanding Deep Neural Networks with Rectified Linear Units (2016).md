---
type: source
source-type: paper
authors: [Raman Arora, Amitabh Basu, Poorya Mianjy, Anirbit Mukherjee]
year: 2016
url: https://consensus.app/papers/details/4a32bf9d9f235821be012619e1a305f0/?utm_source=claude_desktop
doi: 10.48550/arxiv.1611.01491
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [Arora 2016]
---

# Understanding Deep Neural Networks with Rectified Linear Units (2016)

## Summary

Theory paper on what ReLU networks *can represent*. Since ReLU is piecewise linear, a ReLU network computes a continuous piecewise-linear function; this paper quantifies the power of depth in that language.

## Key results

- **Depth-separation gap theorem:** there exist functions representable by a ReLU net with $k^2$ hidden layers and total size $k^3$ such that any net with only $k$ layers needs $\sim \tfrac{1}{2}k^{k+1}$ nodes — a *super-exponential* blowup for being shallow
- Their hard functions form smoothly parameterized families (not isolated pathologies), via zonotope constructions
- Also: a globally-optimal training algorithm for one-hidden-layer ReLU nets (poly in data size, exp in dimension)

## Why it's in the vault

The formal backing for "depth is exponentially powerful with ReLU" — the theoretical companion to ReLU's empirical dominance.

## Concepts extracted

- [[Activation Function]] — ReLU expressivity
- Relevant to a future [[Universal Approximation]] note
