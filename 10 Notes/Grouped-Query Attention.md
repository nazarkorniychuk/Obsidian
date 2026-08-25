---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [GQA, MQA, Multi-Query Attention]
---

# Grouped-Query Attention

The head-sharing lineage of the [[Attention Mechanism]]: reduce the number of **K/V heads** while keeping all query heads, shrinking the [[KV Cache]] (∝ $n_{kv}$) with minimal quality loss. One dial, three settings:

$$\underbrace{n_{kv} = h}_{\text{MHA}} \qquad \underbrace{1 < n_{kv} < h}_{\text{GQA: } h/n_{kv} \text{ queries share each KV head}} \qquad \underbrace{n_{kv} = 1}_{\text{MQA}}$$

Query-head count is untouched — attention *patterns* stay rich; only the **memory being attended to** is shared. Parameters barely change (K/V projections shrink; Q and O dominate), but cache and decode bandwidth drop $h/n_{kv}\times$.

## MQA — the diagnosis and the extreme

- **Diagnosis** ([[Fast Transformer Decoding - One Write-Head is All You Need (2019)|Shazeer 2019]]): incremental decoding is slow because K/V tensors are **re-loaded from memory every step** — bandwidth, not FLOPs. One shared K and V head cuts the tensors $h\times$
- **Results:** much faster decode; **minor but real quality degradation** (small BLEU drop on WMT En-De) — the gap that kept MQA from universal adoption
- **At scale** ([[Efficiently Scaling Transformer Inference (2022)|Pope 2022]]): MQA's smaller cache supports **32× longer contexts** at fixed memory; part of the PaLM-540B serving stack (29 ms/token). Engineering wrinkle: with 1 KV head, tensor parallelism must **replicate** it on every rank — pure MQA shards awkwardly
- Adopters: PaLM, Falcon

## GQA — the practical winner

Two contributions ([[GQA (2023)|Ainslie 2023]]):

1. **The interpolation:** $g$ KV heads, each serving $h/g$ query heads — recovers MQA's speed while restoring quality
2. **The uptraining recipe:** convert an *existing* MHA checkpoint — **mean-pool** each group's K/V heads into one, then continue training for just **5% of original pre-training compute**. No separate inference-optimized pre-training run needed

**Results:** uptrained GQA reaches **quality ≈ MHA at speed ≈ MQA** — the dominated middle of the tradeoff turned out to be nearly free.

**Why $g = 8$ became the industry point:** 8 KV heads ÷ 8-way tensor parallelism = exactly one KV head per GPU rank — no replication, clean sharding (engineering inference; consistent with the Pope 2022 partitioning analysis). Adopters: **LLaMA-2-70B, LLaMA-3 (all sizes), Mistral, Qwen2+, Gemma-2**.

## Beyond heads — the lineage continues

- **Cross-layer (CLA):** share cached K/V between adjacent layers — **2× beyond MQA** at ≈accuracy; a Pareto improvement on the MQA frontier ([[Reducing Transformer KV Cache with Cross-Layer Attention (2024)|Brandon 2024]]); MLKV pushes to 6× below MQA
- **Tying K and V (GTA):** one shared state serves as both key and value — **GQA quality at ~half the KV cache**; sibling GLA makes [[Multi-Head Latent Attention|MLA]]-style latents shardable, up to 2× serving throughput ([[Hardware-Efficient Attention for Fast Decoding (2025)|Zadouri 2025]]). The design axis has shifted from "fewer heads" to **arithmetic intensity per loaded byte**
- **Alternative direction:** compress instead of share — [[Multi-Head Latent Attention]] (better ratio, more complexity)

## Failure modes & limits

- MQA's quality gap is real and grows with compression; GQA at $g{=}1$ *is* MQA — the dial doesn't remove the tradeoff, it exposes it
- Uptraining assumes access to training pipeline + data; mean-pooling initialization is what makes 5% compute suffice
- Head-sharing reduces KV *diversity*: $n_{kv}$ distinct "memory views" instead of $h$ — the (small) representational price, invisible on standard benchmarks but part of why MLA (which keeps per-head K/V after up-projection) can beat GQA at equal cache

## Related

- Variant family of the [[Attention Mechanism]]; exists to shrink the [[KV Cache]]
- Successor by compression: [[Multi-Head Latent Attention]]
- Orthogonal axes compose: GQA + [[Sliding Window Attention]] (Mistral ships both) + quantization

## Sources

- [[Fast Transformer Decoding - One Write-Head is All You Need (2019)]] — MQA
- [[GQA (2023)]] — grouping + uptraining
- [[Efficiently Scaling Transformer Inference (2022)]] — MQA at scale; sharding math
- [[Reducing Transformer KV Cache with Cross-Layer Attention (2024)]] — CLA
- [[Hardware-Efficient Attention for Fast Decoding (2025)]] — GTA/GLA frontier

---
Part of the [[Transformer]] cluster
