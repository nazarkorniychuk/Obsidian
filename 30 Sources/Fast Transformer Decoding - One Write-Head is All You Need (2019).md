---
type: source
source-type: paper
authors: [Noam Shazeer]
year: 2019
url: https://consensus.app/papers/details/60cb511916c25b878675b3faac4379da/?utm_source=claude_desktop
doi: 10.48550/arxiv.1911.02150
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [MQA paper, Shazeer 2019]
---

# Fast Transformer Decoding - One Write-Head is All You Need (2019)

**The MQA paper** (~850 citations). Yet another Shazeer solo paper that set an industry default (see also [[GLU Variants Improve Transformer (2020)]], [[Attention Is All You Need (2017)]], [[Outrageously Large Neural Networks (2017)]]).

## Summary

Diagnosis: incremental decoding is slow because the **K and V tensors must be re-loaded from memory at every step** — a bandwidth cost, not a compute cost. Fix — **Multi-Query Attention**: keep $h$ query heads but **share a single K and V head across all of them**, shrinking the KV tensors (and cache) by $h\times$.

## Key results

- Much faster incremental decoding (bandwidth-bound step time drops with the cache size)
- **Minor quality degradation** vs full MHA (WMT'14 En-De: small but measurable BLEU drop) — the gap that [[GQA (2023)]] later closed

## Impact

Adopted by PaLM and Falcon; [[Efficiently Scaling Transformer Inference (2022)|Pope 2022]] showed MQA's smaller cache enables **32× longer contexts** at scale.

## Concepts extracted

- [[Attention Mechanism]] — KV-cache variant lineage; [[KV Cache]]
