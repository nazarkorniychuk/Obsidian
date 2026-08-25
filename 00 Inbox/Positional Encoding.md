---
type: concept
topics: [deep-learning]
status: seedling
created: 2026-08-24
aliases: [RoPE, positional embedding, position encoding]
---

# Positional Encoding

## Related

- Prerequisite of the [[Attention Mechanism]] — attention is permutation-invariant; position must be injected
- To cover in depth: sinusoidal (Vaswani) vs learned absolute vs relative (Shaw 2018) vs **RoPE** (rotary — the LLM standard) vs ALiBi; long-context extension tricks (position interpolation, NTK-aware scaling, YaRN); why [[Multi-Head Latent Attention|MLA]] needs a decoupled RoPE key
- Sources ready: [[Attention Is All You Need (2017)]] (sinusoidal); RoPE/ALiBi papers still to fetch
- Component of the [[Transformer]] forward pass (step 2 in that note)
