---
type: source
source-type: paper
authors: [Michael Hahn]
year: 2020
url: https://consensus.app/papers/details/2cf546c9f1c6541ebea2a78e7bb68b28/?utm_source=claude_desktop
doi: 10.1162/tacl_a_00306
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Hahn 2020]
---

# Theoretical Limitations of Self-Attention (2020)

*Full title: "Theoretical Limitations of Self-Attention in Neural Sequence Models"* (TACL, ~420 citations).

## Summary

Formal-language analysis of what self-attention **cannot** compute with fixed depth and heads.

## Key results

- Self-attention (hard and soft) **cannot model periodic finite-state languages** (e.g. PARITY — "is the number of 1s even?") **nor hierarchical structure** (Dyck languages — balanced brackets) unless depth or heads **grow with input length**
- Surprising because bracket-matching/hierarchy is exactly what linguistics assumes natural language needs — the resolution: natural language is apparently well-approximated by models too weak for the *idealized* formal hierarchy

## Why it's in the vault

The sharpest "attention has blind spots" theorem — fixed-depth transformers are provably not general sequence computers on unbounded inputs (contrast the arbitrary-precision idealization in [[Attention is Turing-Complete (2021)]]; both are true — the difference is bounded vs unbounded resources). Explains empirical LLM struggles with parity-style state tracking.

## Concepts extracted

- [[Attention Mechanism]] — theory/limits
