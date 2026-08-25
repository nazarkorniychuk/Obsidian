---
type: source
source-type: paper
authors: [Xavier Glorot, Yoshua Bengio]
year: 2010
url: https://consensus.app/papers/details/cbbbec0f3e9d5336b66d26282e6609fd/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Xavier initialization paper, Glorot & Bengio 2010]
---

# Understanding the Difficulty of Training Deep Feedforward Networks (2010)

(AISTATS 2010, ~19,500 citations.)

## Summary

Why did plain deep nets fail before 2006? Monitoring activations and gradients layer-by-layer: (1) **sigmoid saturation** — its non-zero mean drives top layers into saturation plateaus; (2) with naive init, **activation and gradient variances shrink/grow multiplicatively across layers** (Jacobian singular values far from 1). Fix — choose init variance to keep signal variance constant in *both* directions:

$$\text{Var}(W) = \frac{2}{n_{in} + n_{out}} \quad \text{("Xavier/Glorot initialization")}$$

## Key results

- Substantially faster convergence for deep nets with tanh/softsign; diagnosed exactly why sigmoid + naive init stalls
- Established **variance propagation analysis** as *the* method for deriving init schemes — [[Delving Deep into Rectifiers (2015)|He init]] ($2/n_{in}$, accounting for ReLU zeroing half the pre-activations) is this analysis redone for rectifiers

## Concepts extracted

- [[Weight Initialization]] — the founding analysis
