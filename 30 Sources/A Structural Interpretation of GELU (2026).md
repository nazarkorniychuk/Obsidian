---
type: source
source-type: paper
authors: [R. Rossi]
year: 2026
url: https://consensus.app/papers/details/b71a5bf56ad45e55b4b95c2e4175a06b/?utm_source=claude_desktop
doi: 10.48550/arxiv.2607.03664
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-23
aliases: [Rossi 2026, threshold-transmission paper]
---

# A Structural Interpretation of GELU (2026)

*Full title: "A Structural Interpretation of GELU and Threshold-Transmission Activations via the First-Order Loss Function."*

## Summary

Reinterprets GELU: instead of Hendrycks & Gimpel's Bernoulli-gate expectation, GELU is the expected output of a **hard linear gate with a Gaussian-random threshold** — the gate opens once the input clears a latent random cutoff. The decomposition borrows from stochastic inventory theory.

## Key claims

- This view unifies **ReLU, GELU, SiLU/Swish, and hard-swish as one "threshold-transmission" family** — differing only in the threshold's distribution (point mass, Gaussian, logistic, uniform)
- A uniform threshold recovers hard-swish-like piecewise-polynomial gates with tunable/learnable transition width
- Experiments: calibrated uniform-threshold gates are competitive with GELU/ReLU/SiLU on compact vision and language models

## Why it's in the vault

The unifying lens: the whole modern activation zoo is *one* family parameterized by a noise distribution. Makes the GELU ↔ Swish near-identity a theorem instead of a coincidence.

## Concepts extracted

- [[Activation Function]] — the threshold-transmission unification
