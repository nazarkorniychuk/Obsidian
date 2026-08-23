---
type: source
source-type: paper
authors: [Rahul Parhi, Robert D. Nowak]
year: 2019
url: https://consensus.app/papers/details/956924c9cc3a5e0b8aac84222daccf4e/?utm_source=claude_desktop
doi: 10.1109/lsp.2020.3027517
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [Parhi & Nowak 2019]
---

# The Role of Neural Network Activation Functions (2019)

## Summary

A spline-theory answer to "why ReLU, mathematically?" (IEEE Signal Processing Letters). Shows that training single-hidden-layer networks relates to infinite-dimensional optimization over Banach function spaces whose solutions are **splines** — and which spline order you get depends on the activation.

## Key claims

- ReLU and leaky ReLU emerge as *natural* choices: their solutions are the classical piecewise-linear splines
- The framework simultaneously justifies **weight decay** and path-norm regularization, and gives a fresh argument for **skip connections**

## Why it's in the vault

The elegant "ReLU isn't a hack, it's an optimality condition" perspective — good counterweight to purely empirical activation-shopping.

## Concepts extracted

- [[Activation Function]] — theory of why ReLU is principled
