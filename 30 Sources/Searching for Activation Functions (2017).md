---
type: source
source-type: paper
authors: [Prajit Ramachandran, Barret Zoph, Quoc V. Le]
year: 2017
url: https://consensus.app/papers/details/0893b159d5875de2a7e4467b88054675/?utm_source=claude_desktop
doi: 10.48550/arxiv.1710.05941
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-23
aliases: [Swish paper, Ramachandran 2017]
---

# Searching for Activation Functions (2017)

**The Swish paper** (Google Brain). Also circulated under the title *"Swish: a Self-Gated Activation Function"*.

## Summary

Instead of hand-designing an activation, the authors ran **automated search** (exhaustive enumeration + an RL controller) over a space of function compositions, scored by training small networks. The best discovery: $f(x) = x \cdot \text{sigmoid}(\beta x)$, named **Swish** (with $\beta = 1$ it equals SiLU, which had appeared earlier in other contexts). Remarkable because the search *independently converged* on nearly the same curve as [[Gaussian Error Linear Units (2016)|GELU]] — smooth, non-monotonic, self-gated — suggesting that shape is a genuine optimum, not a lucky guess.

## Key results

- Replacing ReLU with Swish: **+0.9% top-1 ImageNet** on Mobile NASNet-A, **+0.6%** on Inception-ResNet-v2
- Gains grow with depth — Swish's advantage is largest on the deepest models tested
- Drop-in replacement: same interface as ReLU, works without retuning hyperparameters

## Impact / adoption

**EfficientNet** and later Google vision models adopted Swish; **MobileNetV3** adopted the cheap approximation *h-swish*. SwiGLU (see [[GLU Variants Improve Transformer (2020)]]) embeds Swish inside a gated FFN — carrying it into LLaMA-family LLMs.

## Caveats

Gains are not universal — see the failed replication [[Benchmarking Comparison of Swish vs Other Activation Functions on CIFAR-10 (2019)]].

## Concepts extracted

- [[Activation Function]] — Swish/SiLU section
