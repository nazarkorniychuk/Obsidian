---
type: source
source-type: paper
authors: [Samira Abnar, Willem Zuidema]
year: 2020
url: https://consensus.app/papers/details/b11b8c02e62b5b0ea58b6f4cf9021912/?utm_source=claude_desktop
doi: 10.18653/v1/2020.acl-main.385
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Abnar 2020, attention rollout]
---

# Quantifying Attention Flow (2020)

(ACL 2020, ~1,300 citations.)

## Summary

The "attention weights are not explanations" paper. Across layers, information from different tokens gets **increasingly mixed** — by mid-network, the residual at position $i$ is a blend of many inputs, so layer-$\ell$ attention weights no longer indicate which *input token* is being used. Proposes two corrections that trace mixing through the whole stack: **attention rollout** (multiply attention matrices, with residual as identity: $\tilde A^{(\ell)} = \prod_{k \le \ell} \tfrac{1}{2}(A^{(k)} + I)$) and **attention flow** (max-flow through the attention graph).

## Key results

- **Raw attention weights correlate poorly with input importance** (measured by ablation and input gradients) beyond the first few layers
- Rollout and flow both correlate substantially better — complementary views

## Why it's in the vault

The methodological warning for every "the model attends to X, therefore it uses X" claim — including informal ones in this vault. Attention visualization needs mixing-aware corrections after layer ~2.

## Concepts extracted

- [[Attention Mechanism]] — interpretability caveat
