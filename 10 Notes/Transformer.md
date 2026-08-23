---
type: concept
topics: [deep-learning]
status: seedling
created: 2026-08-22
aliases: []
---

# Transformer

## The forward pass, in order

1. [[Tokenizer]] → [[Embedding]]
2. [[Positional Encoding]]
3. N × transformer block:
   - [[Attention Mechanism]]
   - [[Feedforward Network]] or [[Mixture of Experts]]
   - [[Layer Normalization]], [[Residual Connection]]
4. [[Unembedding]] → logits → sampling

## Cross-cutting

- [[Activation Function]]
- [[KV Cache]]

## Related

- 
