---
type: source
source-type: paper
authors: [Dan Hendrycks, Kevin Gimpel]
year: 2016
url: https://consensus.app/papers/details/fc847c9b3707508aae7f2859f66a47a9/?utm_source=claude_desktop
doi: 10.48550/arxiv.1606.08415
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-23
aliases: [GELU paper, Hendrycks & Gimpel 2016]
---

# Gaussian Error Linear Units (2016)

**The GELU paper.** Proposes $\text{GELU}(x) = x\Phi(x)$, where $\Phi$ is the standard Gaussian CDF. Exists in two arXiv versions — the earlier one titled *"Bridging Nonlinearities and Stochastic Regularizers with Gaussian Error Linear Units"* emphasizes the derivation.

## Summary

GELU weights inputs *by their value* instead of hard-gating them by sign like ReLU. The derivation is the interesting part: consider a stochastic regularizer that multiplies each pre-activation $x$ by a Bernoulli mask $\sim \text{Bernoulli}(\Phi(x))$ — dropout whose keep-probability depends on the input. The *expected* transformation is exactly $x\Phi(x)$. So GELU is "the deterministic average of input-adaptive dropout" — merging non-linearity and regularization into one function.

## Key results

- Beat ReLU and ELU **across every task tested**: MNIST/CIFAR vision, part-of-speech NLP, TIMIT speech
- The curve is smooth and slightly non-monotonic (dips to ≈ −0.17 near $x \approx -0.75$), giving gradients everywhere — no dead neurons

## Impact / adoption

Ignored for ~2 years, then adopted by **BERT (2018)**, **GPT-1/2/3**, and **ViT** — became the default transformer activation of the pre-LLaMA era. ~7,200 citations.

## Concepts extracted

- [[Activation Function]] — the GELU section
- Gate-based view feeds into [[GLU Variants]] thinking

## My questions

- Why did the *stochastic* interpretation matter if everyone uses the deterministic average?
