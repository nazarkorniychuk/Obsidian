---
type: concept
topics: [deep-learning, optimization]
status: evergreen
created: 2026-08-25
aliases: [optimizer, Muon, Shampoo, optimizer families]
---

# Optimizers

The family of update rules built on [[Gradient Descent]] — what actually turns [[Backpropagation]]'s gradients into weight changes. Deep-dive on the incumbent: [[Adam Optimizer]]. This note maps the whole space.

## ⚡ The family tree

| Optimizer | Update idea | State (bytes/param, fp32) | Status |
| --- | --- | --- | --- |
| SGD | $\theta \mathrel{-}= \eta g$ | 0 | baseline; see [[Gradient Descent]] |
| SGD + momentum | velocity EMA | 4 | ConvNet standard ([[On the Importance of Initialization and Momentum (2013)\|Sutskever 2013]]) |
| AdaGrad → RMSProp | per-param scale from (accumulated → EMA) squared gradients | 4 | the adaptive lineage Adam unified |
| **Adam / AdamW** | momentum ÷ √(second-moment EMA), decoupled decay | 8 | **the LLM incumbent** — [[Adam Optimizer]] |
| Adafactor | factored second moment (row/col) | ≪8 | memory-constrained (T5-era) |
| Lion | sign of momentum interpolation | 4 | discovered-by-search; niche |
| Shampoo / SOAP | full per-matrix preconditioners (Kronecker) | >8 | second-order-lite; strong but heavy |
| **Muon** | orthogonalized momentum (spectral descent) | 4 | **the challenger** — below |

The lineage logic: SGD treats all parameters identically → AdaGrad/RMSProp/Adam add **per-scalar** adaptivity (transformers need it — gradient scales differ wildly across embeddings/LN/attention) → Shampoo/Muon add **per-matrix structure** (a weight matrix's update has geometry a diagonal method can't see).

## Muon — the matrix-geometry turn

**Algorithm:** for each 2D hidden weight matrix, take the momentum matrix $M$ and **orthogonalize** it — approximately replace $M \to UV^\top$ (its polar factor) via ~5 **Newton–Schulz iterations** (a few matmuls, no SVD) — then step. Equivalent view: **steepest descent under the spectral norm** — flatten the update's singular values to 1 so every direction of the matrix moves with equal strength, instead of letting a few dominant directions absorb the step. Non-matrix params (embeddings, gains) stay on AdamW ([[Muon is Scalable for LLM Training (2025)|Liu 2025]]).

**Results:** with weight decay + per-parameter update scaling it works untuned at scale; scaling-law experiments show **~2× computational efficiency vs AdamW** at compute-optimal training; **Moonlight** (3B/16B MoE, 5.7T tokens) trained on it, improving the Pareto frontier — with **half of AdamW's optimizer memory** (momentum only). Follow-ups add neuron-wise second moments (NorMuon: +11% over Muon at 1.1B).

**The honest caveat:** under fair benchmarking, Muon-class gains **shrink as model and data scale grow** with standard constant weight decay — the weight-decay equilibrium controls the angular learning rate, and fixing update/weight norms directly (Hyperball) restores 20–30% token-equivalent speedups at 1.2B ([[Fantastic Pretraining Optimizers II - Hyperball (2026)|Wen 2026]]). Current picture: real but scale-dependent speedup; spectral/orthogonal update geometry is where optimizer research now lives (Dion, GLA-style sharded variants, μP spectral-condition extensions).

## Cross-cutting facts

- **Memory is a first-class constraint:** AdamW's $m, v$ + fp32 master weights ≈ 12–16 bytes/param vs 2 for bf16 weights — why ZeRO sharding, 8-bit optimizer states, and Adafactor exist; Muon's single state is part of its pitch
- **Weight decay ≠ regularization only:** it sets the equilibrium weight norm and thereby the angular learning rate ([[Fantastic Pretraining Optimizers II - Hyperball (2026)]]); its decoupling from adaptivity is the entire AdamW story ([[Decoupled Weight Decay Regularization - AdamW (2017)|Loshchilov & Hutter 2017]])
- **Schedules and warmup** (cosine, WSD, warmup's architectural origin) live in [[Adam Optimizer]]; **hyperparameter transfer across scale** ([[Tensor Programs V - muTransfer (2022)|μP]], now extended to Muon/Shampoo via spectral conditions) in [[Weight Initialization]]
- **Fair comparison is hard:** optimizer papers systematically flatter themselves via tuning asymmetries; the benchmark literature's rule — compare at *matched tuning budget and scale* — is why this note quotes ~2× and 20–30% with their scales attached

## Related

- Base algorithm: [[Gradient Descent]]; incumbent deep-dive: [[Adam Optimizer]]
- Conditioning lens for "why optimizer X wins": [[Neural Tangent Kernel]]
- Interacts with [[Weight Initialization]] (μP), [[Normalization]] (landscape smoothing), [[Layer Normalization]] (warmup)

## Sources

- [[Adam - A Method for Stochastic Optimization (2014)]], [[Decoupled Weight Decay Regularization - AdamW (2017)]] — the incumbent
- [[On the Importance of Initialization and Momentum (2013)]], [[The Marginal Value of Momentum (2023)]] — momentum's scope
- [[Muon is Scalable for LLM Training (2025)]] — the challenger at scale
- [[Fantastic Pretraining Optimizers II - Hyperball (2026)]] — the benchmark referee

---
Part of the foundations cluster.
