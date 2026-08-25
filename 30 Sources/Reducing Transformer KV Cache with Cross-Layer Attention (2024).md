---
type: source
source-type: paper
authors: [William Brandon, Mayank Mishra, Aniruddha Nrusimha, Rameswar Panda, Jonathan Ragan-Kelley]
year: 2024
url: https://consensus.app/papers/details/025de2721d155beda8a8ed8f61e032cf/?utm_source=claude_desktop
doi: 10.48550/arxiv.2405.12981
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [CLA, Brandon 2024]
---

# Reducing Transformer KV Cache with Cross-Layer Attention (2024)

## Summary

Takes KV sharing to the third axis: MQA/GQA share KV heads *within* a layer; **CLA shares K and V between adjacent layers** (layer $\ell{+}1$ attends using layer $\ell$'s cached KV).

## Key results

- **Additional 2× KV cache reduction on top of MQA** at nearly unchanged accuracy (1B and 3B models trained from scratch)
- A **Pareto improvement** over the MQA memory/accuracy frontier — enables longer sequences and larger batches at fixed HBM
- Related: MLKV pushes multi-layer sharing to **6× below MQA** with minimal loss (Zuhri 2024)

## Concepts extracted

- [[Attention Mechanism]] — third axis of KV sharing; [[KV Cache]]
