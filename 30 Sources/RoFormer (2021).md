---
type: source
source-type: paper
authors: [Jianlin Su, Yu Lu, Shengfeng Pan, Ahmed Murtadha, Bo Wen, Yunfeng Liu]
year: 2021
url: https://consensus.app/papers/details/35d8d87ad3ec52c49713f6ed2656104b/?utm_source=claude_desktop
doi: 10.1016/j.neucom.2023.127063
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [RoPE paper, Su 2021]
---

# RoFormer (2021)

*Full title: "RoFormer: Enhanced Transformer with Rotary Position Embedding"* (~6,100 citations).

## Summary

**RoPE**: encode position by *rotating* query/key vectors instead of adding embeddings. Pair up dimensions $(x_{2i}, x_{2i+1})$ and rotate each pair by angle $m\theta_i$ for position $m$, with per-pair frequencies $\theta_i = 10000^{-2i/d}$. The magic identity:

$$(R_m q)^\top (R_n k) = q^\top R_{n-m}\, k$$

— absolute rotations produce attention scores that depend only on the **relative** offset $n{-}m$. Additional properties: valid for any sequence length, inter-token dependency decays with distance, compatible with linear attention.

## Key results

- Consistently beat sinusoidal/learned alternatives on long-text benchmarks
- Zero parameters, zero runtime state (unlike relative-PE tables)

## Impact

**The LLM standard**: LLaMA 1–3, Mistral, Qwen, DeepSeek, Gemma all use RoPE. Its rotation form is also why [[Multi-Head Latent Attention|MLA]] needs a decoupled RoPE key (rotation doesn't commute with MLA's up-projections), and its base parameter $\theta$ became the main long-context tuning knob. Known limits at extreme context: provable loss of locality bias and score consistency as length grows ([[RoPE Distinguishes Neither Positions Nor Tokens (2026)]]).

## Concepts extracted

- [[Positional Encoding]] — the RoPE section
