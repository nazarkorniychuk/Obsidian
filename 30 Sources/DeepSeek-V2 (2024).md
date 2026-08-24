---
type: source
source-type: paper
authors: [DeepSeek-AI]
year: 2024
url: https://consensus.app/papers/details/5772b88c6cd053ce8abb3aca3f5e6719/?utm_source=claude_desktop
doi: 10.48550/arxiv.2405.04434
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [DSv2]
---

# DeepSeek-V2 (2024)

*Full title: "DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model"* (~1,400 citations).

## Summary

First production model built on [[DeepSeekMoE (2024)]] (fine-grained + shared experts) plus **Multi-head Latent Attention** (MLA — compresses the KV cache into latent vectors). **236B total / 21B active**, 128K context.

## Key results

- vs dense DeepSeek-67B: stronger performance with **42.5% lower training cost**, **93.3% smaller KV cache**, **5.76× generation throughput**
- Top-tier open-model performance with only 21B active params

## Concepts extracted

- [[Mixture of Experts]] — adoption; [[Multi-Head Latent Attention]] (future note)
