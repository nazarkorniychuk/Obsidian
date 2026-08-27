---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [LM Head, Output Projection, output embedding]
---

# Unembedding

The final step of the [[Transformer]]: project the last residual state to vocabulary logits and normalize,

$$\text{logits} = W_U\, h_L \in \mathbb{R}^{V}, \qquad p = \text{softmax}(\text{logits}/\tau)$$

with $W_U \in \mathbb{R}^{V \times d}$ (the "LM head"). Loss = cross-entropy against the next token. Sampling (temperature $\tau$, top-p/top-k) operates on this distribution.

## The tying question (shared with [[Embedding]])

- $W_U$ is itself a valid word embedding, and **tying** $W_U = W_E$ cuts parameters and reduced perplexity in the small-model era ([[Using the Output Embedding to Improve Language Models (2016)|Press & Wolf 2016]]); the tied matrix behaves like the *output* embedding of an untied model — prediction is the dominant role
- At scale, **decoupling wins**: notably, a *larger output embedding helps even though it's discarded after pre-training* — it keeps the last layers from over-specializing, making representations more transferable ([[Rethinking Embedding Coupling (2020)|Chung 2020]])

## Structural facts

- **Rank bound:** $\text{rank}(W_U h) \le d \ll V$ — the model can only express next-token distributions living on a $d$-dimensional manifold of the $V$-simplex (the "softmax bottleneck"); with $d{=}4096$, $V{=}128$k, the head is a severe compression
- **The logit lens / early exit:** because the residual stream is additive ([[Identity Mappings in Deep Residual Networks (2016)|identity-mapping math]]), $W_U$ can be applied to *intermediate* layers' states — revealing prediction refining layer by layer. This is the same trick behind reading [[Feedforward Network]] value vectors as vocabulary votes ([[Transformer Feed-Forward Layers Are Key-Value Memories (2021)|Geva 2021]])
- **Geometry pathology:** anisotropic (narrow-cone) output embeddings from likelihood training ([[Representation Degeneration Problem (2019)|Gao 2019]]) connect to end-of-training **output-logit divergence** instabilities; standard mitigations are the z-loss family — penalize $(\log \sum_j e^{z_j})^2$, the same numerics fix as the router z-loss ([[ST-MoE (2022)|Zoph 2022]]) — and logit soft-capping
- **Softmax gate lineage:** the output softmax is the ancestor of every softmax in the architecture — attention scores ([[Attention Mechanism]]) and MoE routers ([[Mixture of Experts]]) reuse the same normalize-and-select mechanism, and inherit the same numerics issues

## Hardware note

Per decoded token the head is one $d \times V$ GEMM + softmax — at $V \approx 128$–256k this rivals a small FFN layer; serving stacks shard the head with tensor parallelism and fuse sampling. Vocabulary growth (see [[Tokenizer]]) is paid here and in [[Embedding]] memory.

## Related

- Mirror of [[Embedding]]; often weight-tied in small models, untied in large
- Applied mid-network = the logit lens; foundation for [[Transformer Feed-Forward Layers Are Key-Value Memories (2021)|FFN value reading]]
- Softmax numerics shared with [[Normalization]]-adjacent stability tricks (z-loss, capping)

## Sources

- [[Using the Output Embedding to Improve Language Models (2016)]] — tying
- [[Rethinking Embedding Coupling (2020)]] — output-embedding size effect
- [[Representation Degeneration Problem (2019)]] — geometry
- [[ST-MoE (2022)]] — the z-loss numerics family

---
Part of the [[Transformer]] cluster
