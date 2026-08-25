---
type: source
source-type: paper
authors: [Yihe Dong, Jean-Baptiste Cordonnier, Andreas Loukas]
year: 2021
url: https://consensus.app/papers/details/571a9de487635fa1929999df3bc048f3/?utm_source=claude_desktop
doi: 10.48550/arxiv.2103.03404
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [rank collapse paper, Dong 2021]
---

# Attention is Not All You Need - Rank Collapse (2021)

*Full title: "Attention is Not All You Need: Pure Attention Loses Rank Doubly Exponentially with Depth"* (~590 citations).

## Summary

Decomposes a stack of pure self-attention layers into a sum of paths through heads, and proves: **without skip connections or FFNs, the output converges to a rank-1 matrix doubly exponentially in depth** — all token representations collapse to the same vector ("token uniformity"). Attention alone is a contraction toward uniformity.

## Key results

- Residual $\|X_L - \text{rank-1}\|$ shrinks like $c^{3^L}$ — after a handful of layers, pure attention has destroyed all token identity
- **Skip connections and the FFN provably stop the collapse** — they are not conveniences but load-bearing counterweights
- Verified empirically on standard transformer variants

## Why it's in the vault

The formal answer to "why does a transformer need the [[Feedforward Network]] and residuals at all?" — attention mixes, but only the other components preserve information under mixing. Later refinements: LayerNorm's role is subtler than "none" (Wu 2024 shows LN+attention can sustain any rank), and local/sparse masks provably slow the collapse.

## Concepts extracted

- [[Attention Mechanism]] — theory/limits; [[Residual Connection]], [[Feedforward Network]]
