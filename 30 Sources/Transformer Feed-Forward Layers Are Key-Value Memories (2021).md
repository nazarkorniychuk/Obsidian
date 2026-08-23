---
type: source
source-type: paper
authors: [Mor Geva, Roei Schuster, Jonathan Berant, Omer Levy]
year: 2021
url: https://consensus.app/papers/details/c9d6cc17e7015b9ca3f1a8164d0cc0ec/?utm_source=claude_desktop
doi: 10.18653/v1/2021.emnlp-main.446
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-23
aliases: [Geva 2021, key-value memories paper]
---

# Transformer Feed-Forward Layers Are Key-Value Memories (2021)

**The interpretability anchor for FFNs** (EMNLP 2021, ~1,600 citations).

## Summary

Reads the FFN $W_2\,\sigma(W_1 x)$ as a key-value memory: rows of $W_1$ are **keys** (pattern detectors over the input), columns of $W_2$ are **values** (distributions over the output vocabulary). Each hidden unit = one memory slot; the FFN output is a weighted sum of value vectors, weights given by key activations.

## Key results

- Opens with the parameter fact everyone now quotes: **FFNs are ~2/3 of a transformer's parameters**
- Keys are **human-interpretable**: experts could identify the triggering textual pattern for most memory slots
- **Depth division of labor:** lower layers detect shallow/surface patterns (n-grams), upper layers semantic ones
- Values in upper layers concentrate probability on the token likely to *follow* the key's pattern — FFN ≈ next-token lookup table
- Layer output = composition of hundreds of active memories, refined via residual stream

## Impact

Foundation of FFN interpretability: knowledge-editing methods (ROME, MEMIT) that rewrite facts by editing FFN weights descend from this view. Follow-up showed FFN and softmax key-value memory are formally equivalent modulo normalization.

## Concepts extracted

- [[Feedforward Network]] — the role/interpretability section
- [[Transformer]]
