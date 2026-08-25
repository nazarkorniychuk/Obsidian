---
type: source
source-type: paper
authors: [Tom B. Brown, et al. (OpenAI)]
year: 2020
url: https://consensus.app/papers/details/d92e91508c0b53498b0f84b3bcfddaaa/?utm_source=claude_desktop
doi: 10.48550/arxiv.2005.14165
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-24
aliases: [GPT-3 paper, Brown 2020]
---

# Language Models are Few-Shot Learners - GPT-3 (2020)

(~62,000 citations.)

## Summary

**GPT-3**: a 175B-parameter decoder-only transformer (10× larger than any prior non-sparse LM), evaluated **without fine-tuning** — tasks specified purely in the prompt (zero/one/few-shot). Established in-context learning as an emergent capability of scale.

## Key results

- Strong few-shot performance across translation, QA, cloze, arithmetic, unscrambling — competitive with fine-tuned SOTA on several
- Human evaluators struggled to distinguish GPT-3 news articles from human-written ones

## Architecture details this vault cites

- FFN: **dense GELU, 4×** ($d=12288$, $d_{ff}=49152$) — see [[Feedforward Network]]
- Attention: **alternating dense and locally-banded sparse attention** layers (the Sparse Transformer pattern) — an early production use of sparse attention, see [[Attention Mechanism]]
- 96 layers, 96 heads, $d_{head}=128$, 2048-token context

## Concepts extracted

- [[Attention Mechanism]], [[Feedforward Network]], [[Activation Function]] — adopter facts
