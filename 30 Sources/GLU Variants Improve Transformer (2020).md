---
type: source
source-type: paper
authors: [Noam Shazeer]
year: 2020
url: https://consensus.app/papers/details/a61336c71bd8535aa0f6696f4a7a7bda/?utm_source=claude_desktop
doi: 10.48550/arxiv.2002.05202
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-23
aliases: [SwiGLU paper, Shazeer 2020]
---

# GLU Variants Improve Transformer (2020)

**The SwiGLU paper.** Four pages, one author, and it changed the FFN of essentially every modern LLM.

## Summary

Takes the Gated Linear Unit (Dauphin et al. 2016) — the product of two linear projections, one squashed through sigmoid — and swaps the sigmoid for other functions: ReGLU (ReLU gate), GEGLU (GELU gate), SwiGLU (Swish gate). Tests them as replacements for the ReLU/GELU [[Feedforward Network]] in a T5-style [[Transformer]], holding parameter count fixed (shrinking $d_{ff}$ by 2/3 to pay for the third matrix).

## Key results

- **GEGLU and SwiGLU beat both ReLU and GELU FFNs** on perplexity in pre-training and on GLUE/SuperGLUE after fine-tuning — consistent, not dramatic, but free at fixed parameters
- Famous closing line: results offered with "no explanation… other than divine benevolence" — the paper claims the win empirically and refuses to theorize

## Impact / adoption

**PaLM, LLaMA 1/2/3, Mistral, Qwen, Gemma** — SwiGLU (or GEGLU) is the de-facto LLM FFN. Post-hoc theory for *why* arrived later: see [[The Devil is in the Condition Numbers (2026)]].

## Concepts extracted

- [[GLU Variants]] — the note this paper anchors
- [[Activation Function]] — the gated-era section
- [[Feedforward Network]] — the layer being redesigned

## My questions

- Is the win from the *gating* or just from having two input projections (more multiplicative interactions)?
