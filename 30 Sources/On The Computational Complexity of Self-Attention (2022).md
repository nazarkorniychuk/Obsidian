---
type: source
source-type: paper
authors: [Feyza Duman Keles, Pruthuvi Mahesakya Wijewardena, Chinmay Hegde]
year: 2022
url: https://consensus.app/papers/details/9003e55e80a35dc08488ae6d93a95016/?utm_source=claude_desktop
doi: 10.48550/arxiv.2209.04881
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Keles 2022, SETH lower bound]
---

# On The Computational Complexity of Self-Attention (2022)

(ALT 2023, ~300 citations.)

## Summary

The hardness result that closes the sub-quadratic dream: **self-attention requires $\Omega(T^2)$ time unless the Strong Exponential Time Hypothesis (SETH) is false** — and the lower bound holds **even for approximate computation** and for multiple attention formulations.

## Key results

- Conditional quadratic lower bound via fine-grained complexity reduction
- Complement: attention *can* be approximated in linear time via finite Taylor series — but with **exponential dependence on the polynomial order** (i.e., not usefully)

## Why it's in the vault

Turns the efficient-attention failure story from empirical observation into (conditional) mathematics: every $o(T^2)$ method **must** be an approximation with a quality tradeoff somewhere — there was never a free lunch to find. The viable escapes are structural: change what's computed (sliding window, linear attention) or change where the bytes live ([[FlashAttention (2022)]] — same $O(T^2)$ FLOPs, fewer HBM reads).

## Concepts extracted

- [[Attention Mechanism]] — theory/limits
