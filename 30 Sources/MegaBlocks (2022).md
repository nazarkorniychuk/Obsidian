---
type: source
source-type: paper
authors: [Trevor Gale, Deepak Narayanan, Cliff Young, Matei Zaharia]
year: 2022
url: https://consensus.app/papers/details/efe4486813c1552a8842ac7bbb8935fc/?utm_source=claude_desktop
doi: 10.48550/arxiv.2211.15841
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Gale 2022]
---

# MegaBlocks (2022)

*Full title: "MegaBlocks: Efficient Sparse Training with Mixture-of-Experts."*

## Summary

The systems problem: GPU kernels want fixed-shape dense matmuls, but MoE routing sends a *variable* number of tokens to each expert. Prior frameworks forced a **capacity factor**: each expert gets a fixed token buffer, overflow tokens are **dropped** (hurting quality), underflow is **padded** (wasting compute). MegaBlocks reformulates expert computation as **block-sparse matrix multiplication** with custom kernels that handle variable expert loads natively.

## Key results

- **Never drops tokens**, no padding waste
- End-to-end training speedups: **up to 40% over Tutel** (the prior state-of-the-art MoE stack), **2.4× over dense trained with Megatron-LM**

## Impact

The dropless-MoE approach became standard (adopted into Databricks' stack, basis of DBRX-style training). Killed the quality-vs-efficiency capacity-factor tradeoff.

## Concepts extracted

- [[Mixture of Experts]] — hardware/systems section
