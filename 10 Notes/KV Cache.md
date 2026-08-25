---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [Key-Value Cache, KV caching]
---

# KV Cache

The decode-time state of an autoregressive [[Transformer]]: the K and V vectors of every previous token, stored so each new token's [[Attention Mechanism]] doesn't recompute them.

**Why it exists:** without caching, generating token $t$ recomputes K,V for all $t{-}1$ prefixes → $O(T^2)$ redundant work per token, $O(T^3)$ per sequence. With the cache: compute K,V once per token, $O(T)$ attention per step. The cache converts compute into **memory** — and that memory became the central constraint of LLM serving.

## ⚡ The numbers

$$\text{bytes/token} = 2 \cdot L \cdot n_{kv} \cdot d_{head} \cdot \text{bytes}_{dtype} \quad (2 = \text{K and V})$$

![[attn-kv-cache.png]]

- LLaMA-2-7B-class (32L, 32 KV heads, $d_{head}$128, fp16): **512 KB/token** → a single 128k-context request = **64 GB** — more than the weights
- Batch × context multiply: 100 concurrent 4k requests at 512 KB/token = 200 GB. **Cache size, not compute, caps serving batch size** — and batch size determines throughput
- Decode arithmetic intensity ≈ 1 FLOP/byte on cached KV — hopelessly below GPU compute:bandwidth ratios (~100:1) → **decode is bandwidth-bound** ([[Efficiently Scaling Transformer Inference (2022)|Pope 2022]]: prefill hits 76% MFU, decode is a different regime entirely, 29 ms/token on PaLM 540B)

## The five shrink axes

| Axis | Method | Reduction | Cost |
| --- | --- | --- | --- |
| Fewer KV heads | [[Fast Transformer Decoding - One Write-Head is All You Need (2019)\|MQA]] / [[GQA (2023)\|GQA]] | $h/n_{kv}$× (32× / 4×) | small→none quality; see [[Grouped-Query Attention]] |
| Low-rank compression | [[DeepSeek-V2 (2024)\|MLA]] | 93.3% | kernel + sharding complexity; see [[Multi-Head Latent Attention]] |
| Cross-layer sharing | [[Reducing Transformer KV Cache with Cross-Layer Attention (2024)\|CLA]] / MLKV | 2× beyond MQA / 6× | ≈none at 2×; grows with sharing factor |
| Windowing | [[Sliding Window Attention]] rolling buffer | cache bounded at $w$ tokens | no long-range attention |
| Eviction | [[StreamingLLM - Attention Sinks (2023)\|StreamingLLM]] | bounded (sinks + window) | lossy: evicted tokens gone |

Plus **quantization** (KV in int8/fp8/int4 — standard practice, multiplies with all of the above).

## Memory management — PagedAttention

- The cache **grows and shrinks dynamically per request**; pre-allocating contiguous max-length buffers wastes most memory (fragmentation + reservation)
- **PagedAttention** ([[PagedAttention - vLLM (2023)|Kwon 2023]]): OS-style virtual-memory paging — fixed-size KV blocks (~16 tokens) allocated on demand via per-request block tables. **Near-zero waste**; **copy-on-write sharing** across requests (shared prefixes, beam search, parallel sampling); vLLM: **2–4× throughput at equal latency**. Now the industry default pattern (vLLM, TGI, TensorRT-LLM, SGLang)

## Eviction and the attention-sink trap

- Obvious idea: keep only the recent window. **It collapses** — perplexity explodes once the *first* tokens leave the cache, because softmax heads park excess attention mass on initial tokens (**attention sinks**) regardless of their content ([[StreamingLLM - Attention Sinks (2023)|Xiao 2023]])
- Fix costs 4 tokens: **keep ~4 sink tokens + rolling window** → stable LM on 4M+ token streams, no fine-tuning, 22.2× faster than window-with-recomputation
- Lesson for all eviction/compression schemes: token importance ≠ attention mass; sinks are structural

## Contrarian corner

- **The cache is formally redundant:** K and V are deterministic projections of the residual stream — caching one residual vector per token (5 KB vs 136 KB on Gemma-3-4B) and recomputing K,V on demand is **bit-identical**, trading bandwidth for FLOPs (KV-Direct; from the same result family as [[Transformer Feed-Forward Layers Are Key-Value Memories (2021)|the residual-stream view]]). Whether recompute beats re-read depends on the compute:bandwidth ratio — increasingly favorable on modern GPUs
- **MoE coupling:** the resident expert pool competes with the KV cache for HBM — a structural serving penalty for [[Mixture of Experts]] at long context ([[The qs Inequality (2026)|Adhinarayanan 2026]])

## Related

- The state shrunk by the whole attention-variant lineage: [[Grouped-Query Attention]] → [[Multi-Head Latent Attention]]; bounded by [[Sliding Window Attention]]; eliminated by [[Linear Attention]] (fixed state — at a recall cost, [[BASED - Recall-Throughput Tradeoff (2024)|Arora 2024]])
- Compute-side counterpart: [[Flash Attention]] (prefill); [[Efficiently Scaling Transformer Inference (2022)|Pope 2022]] (serving math)

## Sources

- [[Fast Transformer Decoding - One Write-Head is All You Need (2019)]] — the bandwidth diagnosis
- [[GQA (2023)]], [[DeepSeek-V2 (2024)]], [[Reducing Transformer KV Cache with Cross-Layer Attention (2024)]] — the shrink lineage
- [[Efficiently Scaling Transformer Inference (2022)]] — prefill/decode economics
- [[PagedAttention - vLLM (2023)]] — memory management
- [[StreamingLLM - Attention Sinks (2023)]] — eviction + sinks
- [[BASED - Recall-Throughput Tradeoff (2024)]] — state-size ↔ recall law
- [[The qs Inequality (2026)]] — MoE/KV HBM competition

---
Part of the [[Transformer]] cluster
