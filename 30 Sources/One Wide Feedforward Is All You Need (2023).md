---
type: source
source-type: paper
authors: [Telmo Pires, António V. Lopes, Yannick Assogba, Hendra Setiawan]
year: 2023
url: https://consensus.app/papers/details/75cc136216345e778b2d485cd168fa76/?utm_source=claude_desktop
doi: 10.48550/arxiv.2309.01826
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [Pires 2023, One Wide FFN]
---

# One Wide Feedforward Is All You Need (2023)

## Summary

Redundancy study of the FFN in encoder-decoder transformers (Apple, machine translation). Tests removing FFNs entirely from some layers and **sharing one FFN across all encoder layers**.

## Key results

- FFNs are **highly redundant across layers**: dropping all decoder FFNs + sharing a single encoder FFN cuts parameters substantially with only a modest accuracy drop
- Re-spending the saved parameters on **one wider shared FFN** (scaling back to original size) **beats the original Transformer Big on both accuracy and latency**
- Conclusion: many per-layer FFNs learn overlapping functions; capacity matters more than per-layer individuality (in MT-scale encoder-decoders)

## Caveat

Demonstrated on translation encoder-decoders, not decoder-only LLMs; LLM-scale replication of FFN sharing remains thin.

## Concepts extracted

- [[Feedforward Network]] — redundancy & sharing section
