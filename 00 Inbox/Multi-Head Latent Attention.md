---
type: concept
topics: [deep-learning]
status: seedling
created: 2026-08-24
aliases: [MLA]
---

# Multi-Head Latent Attention

## Related

- Variant of [[Attention Mechanism]] — KV compression via low-rank latent ($c_t = W_{DKV}h_t$, cache only the latent), not head sharing
- To cover in depth: down/up-projection algebra (folding into $W_Q$/$W_O$), the decoupled-RoPE key and *why* RoPE doesn't commute with the projections, 93.3% KV reduction / 5.76× throughput results, quality-above-MHA claim, GLA (parallel-friendly successor)
- Sources ready: [[DeepSeek-V2 (2024)]], [[DeepSeek-V3 Technical Report (2024)]]
- Interacts with [[Positional Encoding]] and [[KV Cache]]
