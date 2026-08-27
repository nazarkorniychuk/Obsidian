---
type: moc
topics: [deep-learning]
created: 2026-08-25
aliases: [Deep Learning MOC, DL Basics]
---

# Deep Learning Basics

> Parent: [[Home]] · The map of the vault's deep-learning knowledge. Hub notes in **bold**.

## 🧱 Foundations — how anything trains

- **[[Neural Network]]** — the root: expressivity, generalization, over-parameterization
- [[Backpropagation]] — gradient computation; the algorithm's cost structure
- [[Gradient Descent]] — SGD, momentum, why non-convex works
- [[Optimizer]] — the family: SGD → Adam/AdamW → Shampoo → **Muon**; schedules & warmup
- [[AdamW]] — the incumbent deep-dive: moment equations, the decoupling bug, state memory
- [[Normalization]] — the family: BatchNorm → LayerNorm → **RMSNorm** + placement
- [[Weight Initialization]] — Xavier/He, residual scaling, μP transfer
- [[Neural Tangent Kernel]] — the solvable limit; condition numbers; lazy vs feature learning
- [[Activation Function]] — sigmoid → ReLU → GELU/Swish → SwiGLU, with hardware

## 🏛 Transformer core — the forward pass

- **[[Transformer]]** — the hub: block equations, body plans, scaling laws
- [[Tokenizer]] → [[Embedding]] → … → [[Unembedding]] — the vocabulary pipeline
- [[Positional Encoding]] — sinusoidal → relative → RoPE → ALiBi → NoPE
- [[Attention Mechanism]] — the parent note for everything attention
- [[Feedforward Network]] — the parameter store; key-value memory view
- [[Normalization]] · [[Residual Connection]] — the glue (pre/post-LN, rank collapse)

## 👁 Attention family

- [[Multi-Head Attention]] — heads: redundancy, specialization, induction heads
- KV-cache lineage: [[Grouped-Query Attention]] → [[Multi-Head Latent Attention]]
- [[Sliding Window Attention]] · [[Linear Attention]] — the efficiency branches
- [[Flash Attention]] — the IO-aware implementation
- [[KV Cache]] — decode-time state; PagedAttention, sinks, eviction

## ⚖️ Scale & routing

- [[Mixture of Experts]] — routed FFNs: scaling laws, balancing, systems
- [[GLU Variants]] — the gated FFN family

## 🌱 Frontier — queued next

- [[State Space Models]] / [[Mamba]] — post-transformer (sources staged in [[Linear Attention]])
- Quantization · Speculative decoding — the serving pair
- RLHF / DPO / GRPO — post-training cluster → now mapped in [[RL MOC]]
- [[ROME]] · [[MEMIT]] — knowledge editing (inbox)

## 📚 Source library

All papers live in `30 Sources/` with per-paper pages. Top-rated anchors:

```dataview
TABLE authors, year, rating FROM "30 Sources"
WHERE rating = 5
SORT year ASC
```

## Auto-collected (unfiled notes)

```dataview
LIST FROM "10 Notes"
WHERE !contains(file.outlinks, this.file.link) AND file.name != "Home"
```
