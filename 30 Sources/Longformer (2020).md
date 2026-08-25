---
type: source
source-type: paper
authors: [Iz Beltagy, Matthew E. Peters, Arman Cohan]
year: 2020
url: https://consensus.app/papers/details/09350e2d1ba756278bb6d0bac6cc3739/?utm_source=claude_desktop
doi: 10.48550/arxiv.2004.05150
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Beltagy 2020, sliding window attention paper]
---

# Longformer (2020)

*Full title: "Longformer: The Long-Document Transformer"* (~5,800 citations).

## Summary

The canonical **sliding-window attention** paper: each token attends only to a local window of $w$ neighbors → $O(T \cdot w)$ instead of $O(T^2)$; receptive field still grows with depth (layer $\ell$ sees $\ell \cdot w$ tokens, like stacked CNN layers). Adds **task-motivated global attention** on a few designated tokens (e.g. [CLS], question tokens) that attend everywhere and are attended by everyone. Drop-in replacement for full self-attention.

## Key results

- SOTA character-level LM on text8 and enwik8 at publication
- Pretrained Longformer **beats RoBERTa on every long-document task tested**; SOTA WikiHop and TriviaQA; 4096-token context vs BERT's 512

## Impact

The local-window pattern became the standard long-context ingredient: Mistral-7B's sliding window (w=4096), Gemma-2/3's interleaved local-global layers, GPT-3's alternating dense/locally-banded pattern ([[Language Models are Few-Shot Learners - GPT-3 (2020)|Brown 2020]]) are all this idea. Its cousin BigBird added random links with theoretical guarantees.

## Concepts extracted

- [[Attention Mechanism]] — sparse/local variants
