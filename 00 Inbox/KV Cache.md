---
type: concept
topics: [deep-learning]
status: seedling
created: 2026-08-24
aliases: [Key-Value Cache, KV caching]
---

# KV Cache

## Related

- The decode-time state: cached K/V per token = $2 \cdot L \cdot n_{kv} \cdot d_{head} \cdot$ bytes; see chart in [[Attention Mechanism]]
- To cover in depth: why caching (avoid recomputing prefix), prefill vs decode, PagedAttention/vLLM, quantized KV, eviction/compression (H2O, StreamingLLM), cross-layer sharing ([[Reducing Transformer KV Cache with Cross-Layer Attention (2024)|CLA]])
- Shrinking lineage: [[Fast Transformer Decoding - One Write-Head is All You Need (2019)|MQA]] → [[GQA (2023)]] → [[DeepSeek-V2 (2024)|MLA]]
- Competes with MoE expert pool for HBM: [[The qs Inequality (2026)]]
- Serving math: [[Efficiently Scaling Transformer Inference (2022)]]
