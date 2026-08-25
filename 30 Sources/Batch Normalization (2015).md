---
type: source
source-type: paper
authors: [Sergey Ioffe, Christian Szegedy]
year: 2015
url: https://consensus.app/papers/details/4ac9323977f951b49ca6d7ba3ef988be/?utm_source=claude_desktop
doi: 10.57702/o9raffed
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [BatchNorm paper, Ioffe & Szegedy 2015]
---

# Batch Normalization (2015)

*Full title: "Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift"* (~47,500 citations).

## Summary

Normalize each feature **across the mini-batch** (per-channel mean/variance over the batch), then apply learned scale/shift $\gamma, \beta$ — making normalization part of the architecture. Framed as fixing "internal covariate shift" (layer-input distributions drifting during training).

## Key results

- **14× fewer training steps** to match the baseline ImageNet model; enabled much higher learning rates and sloppier initialization; ensemble reached **4.82% top-5** (above human raters); in some cases replaced dropout

## The two corrections history added

1. **The mechanism was wrong:** distributional stability has little to do with it — BN works by making the **optimization landscape significantly smoother** (more predictive, stabler gradients → bigger steps) ([[How Does Batch Normalization Help Optimization (2018)|Santurkar 2018]])
2. **The batch dependence is a liability:** statistics degrade at small batches (GroupNorm exists for this), train/test behavior differs (running statistics), and coupling examples in a batch is unusable for autoregressive decoding — why **transformers use [[Layer Normalization]]-family (per-token) normalization instead**

## Concepts extracted

- [[Normalization]] — the founding (and superseded-in-LLMs) member
