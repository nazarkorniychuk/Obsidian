---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [MLA]
---

# Multi-Head Latent Attention

DeepSeek's [[KV Cache]] compression: instead of *sharing* KV heads ([[Grouped-Query Attention|GQA]]), **compress** each token's K/V information into a small latent vector and cache only that ([[DeepSeek-V2 (2024)|DeepSeek-AI 2024]]).

## The mechanism

**Down-project once, cache the latent:**
$$c_t^{KV} = W^{DKV} h_t \in \mathbb{R}^{d_c}, \qquad d_c \ll h \cdot d_{head}$$
(DeepSeek-V2: $d_c = 512$ vs $d = 5120$ — the latent is ~9% of the hidden size.)

**Up-project at use time, per head:**
$$k_t^{(i)} = W_i^{UK} c_t^{KV}, \qquad v_t^{(i)} = W_i^{UV} c_t^{KV}$$
Each head still gets its *own* K and V (unlike GQA's shared heads) — they're just low-rank functions of one cached latent.

**The absorption trick — no extra decode FLOPs:** the attention score is
$$q_t^\top k_s = q_t^\top W^{UK} c_s = (W^{UK\top} q_t)^\top c_s$$
— fold $W^{UK}$ into the query projection (and $W^{UV}$ into the output projection $W_O$) and attention runs **directly against the cached latents**. The up-projections exist only implicitly.

## The RoPE conflict — and the decoupled key

[[Positional Encoding|RoPE]] applies a *position-dependent rotation* $R_s$ to keys: $k_s = R_s W^{UK} c_s$. Now the absorption fails — $W^{UK}$ can't be folded through $R_s$ because the rotation differs per position ($R_s$ and $W^{UK}$ don't commute). DeepSeek's fix: **split position from content**:
- content path: latent $c_t^{KV}$ (512 dims, no RoPE, absorption works)
- position path: one small **shared rope key** $k_t^R$ (64 dims) carrying RoPE, concatenated at score time

Cache per token per layer = $d_c + d_{rope}$ = **576 dims** (stored once — not doubled for K and V, since the latent serves both) vs MHA's $2 \cdot h \cdot d_{head}$.

## Results

- **93.3% KV cache reduction** and **5.76× maximum generation throughput** vs the dense DeepSeek-67B baseline; 128K context ([[DeepSeek-V2 (2024)]])
- Quality: in DeepSeek's ablations MLA **outperforms full MHA** — low-rank compression acted as regularization rather than a tax; carried unchanged into [[DeepSeek-V3 Technical Report (2024)|DeepSeek-V3]] (671B) alongside [[Mixture of Experts|DeepSeekMoE]]
- Combined with V2's totals: 42.5% cheaper training than the 67B dense predecessor at higher quality

## Trade-offs & the frontier

- **Kernel complexity:** absorbed-latent attention needs custom kernels (FlashMLA) — the [[Flash Attention]] doctrine reapplied to a new dataflow
- **Sharding pain:** the latent is *shared across heads*, so tensor parallelism must **replicate** it per rank (the same problem MQA has). **GLA — Grouped Latent Attention** fixes this: group the latents; matches MLA quality, easier to shard, kernel up to **2× faster than FlashMLA**, up to 2× online-serving throughput ([[Hardware-Efficient Attention for Fast Decoding (2025)|Zadouri 2025]])
- **Prefill cost:** compression saves *decode bandwidth*; prefill still pays full attention compute (that's [[Flash Attention]]'s domain)
- vs GQA: better cache ratio and per-head KV diversity, at the price of engineering complexity — which is why GQA remains the default outside DeepSeek-lineage models

## Related

- Variant of the [[Attention Mechanism]]; the compression axis of [[KV Cache]] reduction (vs sharing: [[Grouped-Query Attention]])
- Forced the [[Positional Encoding]] decoupling — the cleanest demonstration that RoPE's rotation is a real architectural constraint
- Companion of [[Mixture of Experts|DeepSeekMoE]] in DeepSeek-V2/V3

## Sources

- [[DeepSeek-V2 (2024)]] — MLA introduction, 93.3% / 5.76× results
- [[DeepSeek-V3 Technical Report (2024)]] — MLA at 671B
- [[Hardware-Efficient Attention for Fast Decoding (2025)]] — GLA/GTA successors
- [[RoFormer (2021)]] — the rotation that necessitates decoupling

---
Part of the [[Transformer]] cluster
