---
type: source
source-type: paper
authors: [Yufeng Du, et al.]
year: 2026
url: https://consensus.app/papers/details/da346d9fe9fe5b4eb25828dda21e085d/?utm_source=claude_desktop
doi: 10.48550/arxiv.2605.15514
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Du 2026, RoPE long-context limits]
---

# RoPE Distinguishes Neither Positions Nor Tokens (2026)

*Full title: "RoPE Distinguishes Neither Positions Nor Tokens in Long Contexts, Provably."*

## Summary

Content-independent theoretical analysis of RoPE at extreme context lengths.

## Key claims

- As context grows, RoPE attention **loses its locality bias** (no more likely to favor near positions than far ones) and **loses consistency** (a key scoring above another at one position can score below it at another) — both failure probabilities approach 0.5, i.e. coin-flip
- Attention scores can be **unchanged when a key token is moved or even replaced** — failing to distinguish positions *or* tokens
- **Raising the RoPE base** (the standard long-context retrofit: 10k → 500k → 10M) trades token-distinguishing for position-distinguishing — **it cannot preserve both**; multi-head/multi-layer structure doesn't rescue it

## Why it's in the vault

The formal ceiling on the RoPE-scaling program: today's million-token contexts sit inside a provable degradation regime, suggesting genuinely new position mechanisms will be needed. Pairs with signal-processing analyses that find a precision-bounded "Goldilocks zone" for the RoPE base.

## Concepts extracted

- [[Positional Encoding]] — limits of RoPE scaling
