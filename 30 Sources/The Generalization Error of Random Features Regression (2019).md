---
type: source
source-type: paper
authors: [Song Mei, Andrea Montanari]
year: 2019
url: https://consensus.app/papers/details/815222e3fb415ccbbfd80488c6f169fa/?utm_source=claude_desktop
doi: 10.1002/cpa.22008
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [Mei & Montanari 2019, double descent paper]
---

# The Generalization Error of Random Features Regression (2019)

*Full title: "The Generalization Error of Random Features Regression: Precise Asymptotics and the Double Descent Curve"* (CPAM, ~730 citations).

## Summary

The exact mathematics of the phenomenon that broke classical statistics: modern networks hold **more parameters than samples**, can **interpolate pure noise**, and *still generalize*. In the analytically tractable random-features model, derives the **precise asymptotic test error**, exhibiting the **double descent curve**: error rises toward the interpolation threshold (params ≈ samples), then *descends again* as over-parameterization grows.

## Key results

- Sharp characterization of when and why the second descent occurs; over-parameterization acts as implicit regularization
- Puts rigorous foundations under the empirical double-descent observations (Belkin et al.) that upended the bias-variance dogma

## Why it's in the vault

The theory anchor for "more parameters than data, yet it generalizes" — the background assumption of every LLM in this vault, and the reason classical overfitting intuitions ([[Dropout (2014)]]-era) mislead at scale.

## Concepts extracted

- [[Neural Network]] — modern generalization
