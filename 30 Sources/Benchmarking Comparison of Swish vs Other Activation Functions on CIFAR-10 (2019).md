---
type: source
source-type: paper
authors: [Tomasz Szandala]
year: 2019
url: https://consensus.app/papers/details/af57fe91e93c5f3a91ae52857f22a5ad/?utm_source=claude_desktop
doi: 10.1007/978-3-030-19501-4_49
topics: [deep-learning]
status: processed
rating: 2
created: 2026-08-23
aliases: [Szandala 2019]
---

# Benchmarking Comparison of Swish vs Other Activation Functions on CIFAR-10 (2019)

## Summary

An independent replication attempt: benchmarks Swish against ReLU and others on CIFAR-10 with a small CNN.

## Key result

**Swish did *not* outperform ReLU** in this setting — a direct counterexample to the headline claims of [[Searching for Activation Functions (2017)]].

## Why it's in the vault

Not for its own weight (2 citations, small-scale) but as the honesty check: activation gains are **scale- and architecture-dependent**. Swish's advantages were demonstrated on deep ImageNet-scale models; on small CNNs and small datasets, ReLU is typically just as good — and cheaper. Keep this in mind whenever a paper reports a 0.5% win.

## Concepts extracted

- [[Activation Function]] — the caveats
