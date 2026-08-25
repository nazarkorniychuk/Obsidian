---
type: source
source-type: paper
authors: [Catherine Olsson, Nelson Elhage, Neel Nanda, et al. (Anthropic)]
year: 2022
url: https://consensus.app/papers/details/80670b235c8053ad9e3c5cc2ef242c13/?utm_source=claude_desktop
doi: 10.48550/arxiv.2209.11895
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Olsson 2022, induction heads paper]
---

# In-context Learning and Induction Heads (2022)

(Anthropic transformer-circuits work, ~980 citations.)

## Summary

**Induction heads**: attention heads implementing a match-and-copy algorithm — on seeing `[A][B] … [A]`, attend back to the token *after* the previous `[A]` and predict `[B]`. Requires two cooperating heads across layers (a "previous-token head" writes position-shifted info; the induction head matches on it).

## Key results

- Induction heads **emerge abruptly during training, exactly at the phase change** where in-context learning ability jumps (a visible bump in the loss curve)
- Six lines of evidence that induction heads are **the mechanistic source of most in-context learning** — causal in small attention-only models, correlational at scale
- Follow-ups: emergence driven by three interacting subcircuits; "n-gram head" generalizations; hard-wiring such heads improves LM perplexity

## Why it's in the vault

The flagship positive answer to "what does an individual attention head *do*?" — and the mechanistic grounding for the emergence of in-context learning observed at scale in [[Language Models are Few-Shot Learners - GPT-3 (2020)|GPT-3]].

## Concepts extracted

- [[Multi-Head Attention]] — head specialization at its sharpest
