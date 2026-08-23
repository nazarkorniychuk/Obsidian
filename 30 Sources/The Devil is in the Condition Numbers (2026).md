---
type: source
source-type: paper
authors: [Xingyu Lyu, et al.]
year: 2026
url: https://consensus.app/papers/details/7eb498afe2f45d0aa3f490928875e399/?utm_source=claude_desktop
doi: 10.48550/arxiv.2605.20749
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-23
aliases: [GLU NTK paper]
---

# The Devil is in the Condition Numbers (2026)

*Full title: "The Devil is in the Condition Numbers: Why is GLU Better than non-GLU Structure?"*

## Summary

The long-awaited answer to Shazeer's "divine benevolence" shrug in [[GLU Variants Improve Transformer (2020)]]. Analyzes two-layer GLU networks in the neural tangent kernel (NTK) regime.

## Key claims

- The gating structure **reshapes the NTK spectrum**: smaller condition number, more compact eigenvalue distribution
- This predicts and explains **faster convergence** for GLU models, including the characteristic loss-curve crossing between GLU and non-GLU runs
- Empirically (ViT, GPT-2): GLU's effect on the *generalization gap* is limited — the benefit is **optimization speed, not generalization**

## Why it matters

Reframes the SwiGLU story: modern LLMs don't use gated FFNs because they generalize better, but because they *train faster at fixed compute* — which at LLM scale is the same thing as being better.

## Concepts extracted

- [[GLU Variants]], [[Activation Function]] — the "why gating wins" explanation

## My questions

- Does NTK-regime analysis actually transfer to finite-width LLMs, or is this another idealization?
