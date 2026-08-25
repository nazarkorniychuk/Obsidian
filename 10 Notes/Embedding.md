---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [Embedding Matrix, Token Embedding, input embedding]
---

# Embedding

The learned map from [[Tokenizer]] ids to vectors: a matrix $W_E \in \mathbb{R}^{V \times d}$ whose row $i$ is token $i$'s vector. "Lookup" is just multiplication by a one-hot vector — the embedding is an ordinary linear layer whose input happens to be one-hot, trained end-to-end by [[Backpropagation]] like everything else.

$$x_t = W_E[\text{id}_t] \;(+\; \text{position, if absolute PE — modern models use [[Positional Encoding|RoPE]] in attention instead})$$

## ⚡ Parameter arithmetic — when embeddings matter

$|W_E| = V \cdot d$:

| Model | $V \times d$ | Embedding params | Share of model |
| --- | --- | --- | --- |
| GPT-2-small | 50,257 × 768 | 38.6M | **≈31%** (of 124M) |
| LLaMA-2-7B | 32,000 × 4096 | 131M | ≈1.9% |
| Gemma-7B | 256,000 × 3072 | 786M | ≈9% |

**Weight tying** = use **one shared $V \times d$ matrix** for both token↔vector maps: the input [[Embedding]] $W_E$ (job: *represent* a token) and the output [[Unembedding]] $W_U$ (job: *score* the hidden state against each token, $\text{logit}_i = W_U[i]\cdot h$). Setting $W_U = W_E$ — literally the same tensor, gradients from both roles accumulating into it — deletes an entire $V \times d$ matrix, and is coherent because both matrices encode "one vector per token" with similar-word structure ([[Using the Output Embedding to Improve Language Models (2016)|Press & Wolf 2016]]).

**The regime split:** in small models embeddings dominate (GPT-2: 31%) → tying reclaims that budget *and* improves perplexity (each token's single vector trains on both its input and target occurrences — a data-efficiency bonus). In large models embeddings are a rounding error (~2%) → savings are irrelevant and **untying's specialization wins** ([[Rethinking Embedding Coupling (2020)|Chung 2020]]): the two jobs diverge — under tying, output gradients dominate and the shared matrix becomes an output vector doing double duty; decoupling, shrinking the input embedding, and spending the savings on layers is dramatically better downstream at equal parameters, especially multilingual. Rule: **tie small, untie large** (GPT-2 tied; LLaMA-family untied; Gemma ties despite its size because its 256k vocab puts embeddings back in the "dominant" regime).

## What the vectors become

- Row vectors start as *context-free* token identities; the [[Transformer]]'s job is to contextualize them — by the top layer, the residual stream at position $t$ is a mixture of the whole prefix ([[Quantifying Attention Flow (2020)|Abnar 2020]])
- **Semantic structure emerges** (similar tokens → nearby vectors) but with a caveat: likelihood training drives embeddings into a **narrow anisotropic cone** — average pairwise cosine similarity is high for *all* tokens, inflating naive similarity measures ([[Representation Degeneration Problem (2019)|Gao 2019]]). Any embedding-space analysis should account for the cone
- Under tying, one matrix serves two masters — and gradient analysis shows the shared matrix is shaped **primarily by the output role** (output gradients dominate early), degrading its input-representation quality; mechanistic evidence for why tying hurts at scale (Lopardo 2026, follow-up to [[Using the Output Embedding to Improve Language Models (2016)|Press & Wolf]])

## Related

- Input side of the pair with [[Unembedding]] (weight tying question lives across both)
- Consumes [[Tokenizer]] output; vocabulary size set there
- First write into the residual stream that [[Attention Mechanism]] and [[Feedforward Network]] then edit ([[Identity Mappings in Deep Residual Networks (2016)|residual-stream view]])

## Sources

- [[Using the Output Embedding to Improve Language Models (2016)]] — tying
- [[Rethinking Embedding Coupling (2020)]] — untying at scale
- [[Representation Degeneration Problem (2019)]] — anisotropy

---
Part of the [[Transformer]] cluster
