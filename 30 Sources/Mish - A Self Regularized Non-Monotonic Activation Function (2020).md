---
type: source
source-type: paper
authors: [Diganta Misra]
year: 2020
url: https://consensus.app/papers/details/45b489ac1e2a5e44a7591c1e932bcc1e/?utm_source=claude_desktop
doi: 10.5244/c.34.191
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [Mish paper]
---

# Mish - A Self Regularized Non-Monotonic Activation Function (2020)

## Summary

Proposes $\text{Mish}(x) = x \tanh(\text{softplus}(x))$ — a member of the Swish family (smooth, non-monotonic, self-gated), with a slightly wider negative dip and a first-derivative shape the author argues acts as a built-in regularizer that smooths the loss landscape.

## Key results

- **ImageNet:** ≈ +1% top-1 over ReLU on ResNet-50 at identical hyperparameters
- **MS-COCO detection:** +2.1% AP₅₀ over Leaky ReLU on YOLOv4 (CSP-DarkNet-53)
- Consistent wins over both ReLU and Swish across benchmark sweeps (BMVC 2020, ~1,100 citations)

## Impact / adoption

**YOLOv4/YOLOv5-era detection models** are the flagship adopters. Never crossed into transformers — GELU/SwiGLU had already won there, and Mish costs more (tanh + softplus).

## Concepts extracted

- [[Activation Function]] — smooth self-gated family
