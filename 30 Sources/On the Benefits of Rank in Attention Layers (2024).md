---
type: source
source-type: paper
authors: [Noah Amsel, Gilad Yehudai, Joan Bruna]
year: 2024
url: https://consensus.app/papers/details/a4606d188e1d56678d5f136443fbe67e/?utm_source=claude_desktop
doi: 10.48550/arxiv.2407.16153
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Amsel 2024, rank vs heads]
---

# On the Benefits of Rank in Attention Layers (2024)

## Summary

Theory on the un-questioned convention $d_{head} = d/h$: are many low-rank heads as expressive as few full-rank ones?

## Key results

- **No — dramatic tradeoffs exist**: a natural target function representable by a **single full-rank head** at any context length cannot be approximated by low-rank attention unless the head count is **exponential** in the embedding dimension
- Depth partially rescues low rank for *short* contexts; for long contexts full rank is conjectured necessary
- Validated with experiments on standard transformers

## Why it's in the vault

The formal caveat under the standard MHA recipe: splitting $d$ into $h$ rank-$d/h$ heads is a *real* expressivity restriction, not a free reparameterization — relevant to why [[Multi-Head Latent Attention|MLA]]-style designs that restore per-head expressiveness can beat head-sharing at equal cache.

## Concepts extracted

- [[Multi-Head Attention]] — the rank-vs-heads tradeoff
