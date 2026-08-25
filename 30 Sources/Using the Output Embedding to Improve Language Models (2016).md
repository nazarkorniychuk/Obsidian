---
type: source
source-type: paper
authors: [Ofir Press, Lior Wolf]
year: 2016
url: https://consensus.app/papers/details/53aca524f0ff505fbc3b15ceb36a41f5/?utm_source=claude_desktop
doi: 10.18653/v1/e17-2025
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [weight tying paper, Press & Wolf 2016]
---

# Using the Output Embedding to Improve Language Models (2016)

**The weight-tying paper** (~840 citations; same Ofir Press as [[ALiBi (2021)]]).

## Summary

The output projection (unembedding) of a language model is itself a valid word embedding — so **tie it to the input embedding** (one shared $V \times d$ matrix for both directions). Analysis of the update rules shows the tied matrix evolves like the *output* embedding of an untied model.

## Key results

- **Significant perplexity reductions** across LM architectures
- Halves the embedding parameter count — translation models shrink to **less than half** their size without quality loss

## Impact & the modern reversal

Standard in GPT-1/GPT-2 and most small models. At scale the practice reversed: decoupled embeddings allow better parameter allocation and more transferable representations (larger *output* embeddings help even though they're discarded after pre-training — Chung et al. 2020), and mechanistic follow-ups show tying **biases the shared matrix toward the output space** (output gradients dominate early training), degrading early-layer input processing — part of why LLaMA-class models leave them untied. Rule of thumb: tie when embeddings dominate the parameter budget (small models), untie at scale.

## Concepts extracted

- [[Embedding]], [[Unembedding]] — the tying question
