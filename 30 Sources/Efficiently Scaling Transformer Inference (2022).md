---
type: source
source-type: paper
authors: [Reiner Pope, Sholto Douglas, Aakanksha Chowdhery, Jacob Devlin, James Bradbury, Anselm Levskaya, Jonathan Heek, Kefan Xiao, Shivani Agrawal, Jeff Dean]
year: 2022
url: https://consensus.app/papers/details/7a75b56604b25d108824b223e237da1a/?utm_source=claude_desktop
doi: 10.48550/arxiv.2211.05102
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Pope 2022]
---

# Efficiently Scaling Transformer Inference (2022)

## Summary

The canonical LLM-serving engineering study (Google, PaLM 540B on TPU v4): an analytical cost model for picking multi-dimensional partitioning (tensor/expert/pipeline) given latency targets, plus the empirical case for MQA at scale.

## Key results

- **MQA's smaller KV cache enables scaling to 32× longer context lengths** vs MHA under the same memory — the concrete serving argument for the [[Fast Transformer Decoding - One Write-Head is All You Need (2019)|MQA]]/[[GQA (2023)|GQA]] lineage
- PaLM 540B: **29 ms/token generation latency** (int8 weights, low batch) and **76% model-FLOPS utilization** at large-batch prefill
- Formalizes the **prefill vs decode split**: prefill is compute-bound (batch over sequence), decode is bandwidth-bound (one token at a time) — different optimal partitionings for each

## Concepts extracted

- [[Attention Mechanism]] — hardware section; [[KV Cache]]
