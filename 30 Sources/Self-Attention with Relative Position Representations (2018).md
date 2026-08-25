---
type: source
source-type: paper
authors: [Peter Shaw, Jakob Uszkoreit, Ashish Vaswani]
year: 2018
url: https://consensus.app/papers/details/9ca4c2c48cbf515885f60c5fd7c19975/?utm_source=claude_desktop
doi: 10.18653/v1/n18-2074
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Shaw 2018, relative position representations]
---

# Self-Attention with Relative Position Representations (2018)

(NAACL 2018, ~2,900 citations.)

## Summary

The first relative positional encoding for transformers: instead of adding absolute positions to the input, inject **learned embeddings of the clipped relative distance** $j - i$ directly into the attention computation (added to keys, optionally values). Distances beyond $\pm k$ share one embedding.

## Key results

- **+1.3 BLEU** (En-De) and **+0.3 BLEU** (En-Fr) over absolute sinusoidal positions on WMT'14
- Combining relative + absolute gave *no further gain* — relative information is what attention actually needs

## Impact

Founded the relative-PE lineage: T5's bucketed relative biases, ALiBi's fixed linear biases, and RoPE (which achieves relativity *multiplicatively*) are all descendants of this observation.

## Concepts extracted

- [[Positional Encoding]] — the relative turn
