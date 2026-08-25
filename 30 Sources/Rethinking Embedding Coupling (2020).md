---
type: source
source-type: paper
authors: [Hyung Won Chung, Thibault Févry, Henry Tsai, Melvin Johnson, Sebastian Ruder]
year: 2020
url: https://consensus.app/papers/details/27540784c4305a999bb594abbacf532a/?utm_source=claude_desktop
doi: 10.48550/arxiv.2010.12821
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Chung 2020, decoupled embeddings]
---

# Rethinking Embedding Coupling (2020)

*Full title: "Rethinking embedding coupling in pre-trained language models"* (ICLR 2021).

## Summary

Re-evaluates weight tying for the pre-train/fine-tune era: **decouple** input and output embeddings and re-allocate the freed parameters.

## Key results

- Shrinking the *input* embedding and spending the savings on transformer layers → **dramatically better downstream performance at equal fine-tuning parameter count** (especially multilingual, where vocabularies are huge)
- **Larger output embeddings help even though they are thrown away after pre-training** — they prevent the last layers from over-specializing to the pre-training task, making representations more general and transferable
- Basis of practical untied designs (mT5-family lineage)

## Why it's in the vault

The counter-argument to [[Using the Output Embedding to Improve Language Models (2016)|weight tying]]: input and output embeddings do *different jobs*, and at scale the flexibility beats the parameter savings.

## Concepts extracted

- [[Embedding]], [[Unembedding]] — the untying case
