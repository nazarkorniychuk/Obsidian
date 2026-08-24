---
type: source
source-type: paper
authors: [Albert Q. Jiang, et al. (Mistral AI)]
year: 2024
url: https://consensus.app/papers/details/bdf72c49bbee5dd7b7a545fb12775a6e/?utm_source=claude_desktop
doi: 10.48550/arxiv.2401.04088
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Mixtral paper, Mixtral 8x7B]
---

# Mixtral of Experts (2024)

**The open-weights MoE breakout** (~2,100 citations).

## Summary

Mistral-7B architecture with each FFN replaced by **8 expert FFNs + top-2 router**. Every token uses 2 of 8 experts per layer (experts can differ per layer and per timestep).

## Key results

- **47B total parameters, 13B active** per token at inference
- **Outperforms or matches LLaMA-2-70B and GPT-3.5** across benchmarks, with large margins on math, code, and multilingual tasks — at ~5× fewer active params than LLaMA-2-70B
- 32k context; instruct version beat GPT-3.5-Turbo, Claude-2.1, and Gemini Pro on human evals
- Apache 2.0 — proved frontier-competitive MoE in open weights

## Why it's in the vault

The concrete active-vs-total data point in [[Feedforward Network]] and [[Mixture of Experts]]: MoE buys 70B-class quality at 13B-class inference compute.

## Concepts extracted

- [[Mixture of Experts]], [[Feedforward Network]] — MoE adoption
