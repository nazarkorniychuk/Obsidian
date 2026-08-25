---
type: source
source-type: paper
authors: [Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun]
year: 2015
url: https://consensus.app/papers/details/ade4330e3a2b5f4d9de7b4524092f658/?utm_source=claude_desktop
doi: 10.1109/cvpr.2016.90
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [ResNet paper, He 2015]
---

# Deep Residual Learning for Image Recognition (2015)

**The ResNet paper** (~237,000 citations — the most cited paper in this vault).

## Summary

Deeper plain networks *degrade* — not from overfitting but from optimization failure (training error rises with depth). Fix: make each block learn a **residual** $F(x)$ and output $F(x) + x$ via an identity **skip connection**. Learning "the change to make" is easier than learning the whole mapping; if a layer should do nothing, it only has to drive $F \to 0$.

## Key results

- **152-layer** networks trained cleanly (8× deeper than VGG, lower complexity); plain nets of that depth fail
- **3.57% top-5 ImageNet error** — 1st place ILSVRC 2015 (also 1st in detection, localization, COCO); +28% relative COCO improvement from depth alone
- CIFAR experiments up to **1000+ layers**; follow-up ([[Identity Mappings in Deep Residual Networks (2016)|He 2016]]) shows *pure identity* skips (no gating/scaling) give the cleanest signal path — forward and backward signals propagate directly between any two blocks — training a 1001-layer net to 4.62% CIFAR-10 error

## Impact

The single most important architectural idea of the deep-learning era after the CNN itself. The [[Transformer]] adopted residual connections around every sublayer from day one — enabling the depth the whole LLM stack relies on, and the "residual stream" framing of interpretability.

## Concepts extracted

- [[Residual Connection]] — origin + mechanism
