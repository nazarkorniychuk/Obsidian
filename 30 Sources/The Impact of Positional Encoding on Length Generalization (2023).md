---
type: source
source-type: paper
authors: [Amirhossein Kazemnejad, Inkit Padhi, Karthikeyan Natesan Ramamurthy, Payel Das, Siva Reddy]
year: 2023
url: https://consensus.app/papers/details/b56c0628ae34558cbd38c179ca4c24d6/?utm_source=claude_desktop
doi: 10.48550/arxiv.2305.19466
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [NoPE paper, Kazemnejad 2023]
---

# The Impact of Positional Encoding on Length Generalization (2023)

(NeurIPS 2023, ~410 citations.)

## Summary

Systematic comparison of five positional schemes — learned absolute (APE), T5-relative, ALiBi, RoPE, and **NoPE (no positional encoding at all)** — on length generalization in *decoder-only* transformers, over reasoning/math tasks.

## Key results

- **The popular methods (RoPE, ALiBi, APE) are all poor at downstream length generalization**
- **NoPE outperforms every explicit scheme** — while costing nothing. Decoder-only models don't strictly need positional encoding: the **causal mask itself breaks permutation symmetry** (each token sees a different-length prefix)
- Theory: NoPE can represent both absolute and relative encodings; trained with SGD it behaves like T5's relative attention patterns

## Why it's in the vault

The field's most counterintuitive PE result: position information is partially *emergent* in causal transformers, and explicit encodings are a prior, not a necessity. (Caveat: shown on small/medium models and synthetic tasks; frontier LLMs still ship RoPE.)

## Concepts extracted

- [[Positional Encoding]] — the NoPE surprise
