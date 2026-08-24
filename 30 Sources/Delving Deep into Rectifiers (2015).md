---
type: source
source-type: paper
authors: [Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun]
year: 2015
url: https://consensus.app/papers/details/9af0f092042e576b944f5725ec636678/?utm_source=claude_desktop
doi: 10.1109/iccv.2015.123
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-24
aliases: [He 2015, PReLU paper]
---

# Delving Deep into Rectifiers (2015)

*Full title: "Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification"* (ICCV 2015, ~21,000 citations).

## Summary

Two contributions: **PReLU** — Leaky ReLU with the negative slope $\alpha$ *learned* per channel (near-zero extra cost) — and **He/Kaiming initialization**, a weight-init scheme derived specifically for rectifier non-linearities that enables training very deep rectified nets from scratch.

## Key results

- **4.94% top-5 ImageNet error** — the **first result to surpass human-level performance** (5.1%); 26% relative improvement over GoogLeNet
- He initialization became the standard init for ReLU-family networks (default in PyTorch)

## Concepts extracted

- [[Activation Function]] — PReLU
- Future note: [[Weight Initialization]]
