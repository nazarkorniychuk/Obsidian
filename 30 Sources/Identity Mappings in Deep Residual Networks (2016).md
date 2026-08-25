---
type: source
source-type: paper
authors: [Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun]
year: 2016
url: https://consensus.app/papers/details/b8c4098ad3fc5f37bfa989491ff7cb25/?utm_source=claude_desktop
doi: 10.1007/978-3-319-46493-0_38
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [He 2016, pre-activation ResNet]
---

# Identity Mappings in Deep Residual Networks (2016)

(ECCV 2016, ~11,000 citations.)

## Summary

The theory companion to [[Deep Residual Learning for Image Recognition (2015)|ResNet]]: analyze signal propagation through residual blocks. With **pure identity** skip connections and pre-activation ordering, the forward signal from any block $l$ to any deeper block $L$ is
$$x_L = x_l + \sum_{i=l}^{L-1} F(x_i)$$
— *additive*, never multiplicative — and the backward gradient contains an **unattenuated identity term**: $\frac{\partial \mathcal{L}}{\partial x_l} = \frac{\partial \mathcal{L}}{\partial x_L}\big(1 + \tfrac{\partial}{\partial x_l}\sum F\big)$. No vanishing along the skip path, at any depth.

## Key results

- Ablations: *any* modification of the skip path (scaling, gating, 1×1 conv, dropout) hurts — identity is optimal
- Pre-activation ordering → **1001-layer ResNet trains to 4.62% CIFAR-10 error** (better than the 110-layer original)

## Why it's in the vault

The formal basis of the **residual stream** view: the network is a stream that blocks *add into*, which is exactly how [[Transformer]] interpretability (Geva's key-value memories, logit lens) reads the architecture — and why pre-LN transformers (LN off the skip path) train stably.

## Concepts extracted

- [[Residual Connection]] — the identity math
