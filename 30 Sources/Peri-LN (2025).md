---
type: source
source-type: paper
authors: [Jeonghoon Kim, et al.]
year: 2025
url: https://consensus.app/papers/details/0daee1c2ca84517d97ce035e2272a812/?utm_source=claude_desktop
doi: 10.48550/arxiv.2502.02732
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Kim 2025, peri-layer normalization]
---

# Peri-LN (2025)

*Full title: "Peri-LN: Revisiting Normalization Layer in the Transformer Architecture."*

## Summary

Documents and analyzes a third normalization placement that several open models had **silently adopted**: normalize **peripherally around each sublayer** — both its input *and* its output — while keeping the main residual path clean:

$$h' = h + \text{Norm}_{out}\big(F(\text{Norm}_{in}(h))\big)$$

(Gemma-2's sandwich norm is the flagship instance.)

## Key results

- Analytical account of how each placement (pre/post/peri) shapes **activation-variance growth and gradient propagation** with depth
- Experiments to **3.2B params**: Peri-LN consistently achieves more balanced variance growth, steadier gradients, and better convergence stability than pre- or post-LN

## Why it's in the vault

The current answer to the placement dilemma: pre-LN is stable but lets stream variance grow (curse of depth); post-LN is effective but unstable; peri-LN bounds the *output* of each branch before it enters the stream — attacking pre-LN's pathology at the source.

## Concepts extracted

- [[Normalization]], [[Normalization]] — the placement frontier
