---
type: source
source-type: paper
authors: [Shiv Ram Dubey, Satish Kumar Singh, Bidyut Baran Chaudhuri]
year: 2021
url: https://consensus.app/papers/details/adb6f177ec36516981daac7d3e9ea3d5/?utm_source=claude_desktop
doi: 10.1016/j.neucom.2022.06.111
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-23
aliases: [Dubey survey]
---

# Activation Functions in Deep Learning - A Comprehensive Survey and Benchmark (2021)

## Summary

The reference survey (Neurocomputing, ~1,200 citations). Taxonomizes activations into sigmoid/tanh-based, ReLU-based, ELU-based, and *learned* families; catalogs properties (range, monotonicity, smoothness); and — the useful part — runs an **18-activation benchmark across different architectures and data types** with released code.

## Key takeaways

- No single activation wins everywhere: the best choice **interacts with architecture and data modality**
- ReLU-family remains extremely competitive on CNNs; smooth self-gated functions (Swish/GELU/Mish) win more often on deeper and attention-based models
- Many exotic published activations fail to replicate outside their home paper — publication bias is rampant in this literature

## Use in this vault

Serves as the neutral referee for cross-activation claims in [[Activation Function]], balancing the self-reported results of the original papers.

## Concepts extracted

- [[Activation Function]] — "which is better for what" guidance
