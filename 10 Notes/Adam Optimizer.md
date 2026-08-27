---
type: concept
topics: [deep-learning, optimization]
status: evergreen
created: 2026-08-25
aliases: [Adam, AdamW, adaptive optimizers]
---

# Adam Optimizer

The de-facto optimizer of the transformer era ([[Adam - A Method for Stochastic Optimization (2014)|Kingma & Ba 2014]]): [[Gradient Descent]] with per-parameter adaptive step sizes from running moment estimates.

## The algorithm

$$m_t = \beta_1 m_{t-1} + (1{-}\beta_1)\,g_t \qquad v_t = \beta_2 v_{t-1} + (1{-}\beta_2)\,g_t^2$$
$$\hat m_t = \frac{m_t}{1-\beta_1^t}, \quad \hat v_t = \frac{v_t}{1-\beta_2^t} \qquad \theta_{t+1} = \theta_t - \eta\,\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon}$$

- $m$ = momentum (EMA of gradients); $v$ = per-parameter scale (EMA of squared gradients); the ratio ≈ **signal-to-noise-normalized step** — every parameter moves at comparable speed regardless of its raw gradient magnitude (invariance to diagonal rescaling)
- **Bias correction** ($1-\beta^t$ terms): the EMAs start at zero and would underestimate early; the correction divides it out exactly
- Defaults $\beta_1{=}0.9$, $\beta_2{=}0.999$, $\epsilon{=}10^{-8}$; **LLM practice: $\beta_2 = 0.95$** (0.999 reacts too slowly to the abrupt gradient-scale shifts of large-batch LM training → instability spikes)
- **Why transformers need it:** gradient scales differ wildly across parameter types (embeddings touched sparsely per step, LN gains, attention vs FFN) and the noise is heavy-tailed — a single global $\eta$ (SGD) can't serve all; Adam's per-parameter normalization can

## AdamW — the weight-decay bug fix

For SGD, $L_2$ penalty ≡ weight decay. **Under Adam they differ**: the $L_2$ gradient $\lambda\theta$ passes through the $\sqrt{\hat v}$ normalizer, so heavily-updated parameters get *less* regularization — backwards. **AdamW decouples**: $\theta_{t+1} = \theta_t - \eta(\hat m_t/(\sqrt{\hat v_t}+\epsilon) + \lambda\theta_t)$ — decay applied outside the adaptive machinery ([[Decoupled Weight Decay Regularization - AdamW (2017)|Loshchilov & Hutter 2017]]). Results: $\lambda$ and $\eta$ become independently tunable; generalization gap to momentum-SGD closes. Later theory: AdamW ≈ $\ell_\infty$-constrained optimization ($\|\theta\|_\infty \lesssim 1/\lambda$). **Every modern LLM config reads "AdamW".**

## The Adam-specific warmup reason

Schedules (warmup shapes, cosine vs WSD, peak-$\eta$ scaling) are optimizer-agnostic and live in [[Optimizers]] → Schedules. Adam adds one *specific* reason warmup helps: the second-moment EMA $v$ is **noisy and biased small in the first steps** (few samples), so early adaptive steps $\hat m/\sqrt{\hat v}$ can be violently large in low-$v$ coordinates — ramping $\eta$ shields against exactly this, complementing the architectural (post-norm) warmup story.

## Costs & systems reality

- **State: 2 extra fp32 tensors per parameter** ($m, v$) → with fp32 master weights, optimizer state ≈ **12 bytes/param vs 2 for bf16 weights** — the dominant training-memory item, motivating ZeRO-style sharding across GPUs and 8-bit optimizer states
- Frontier alternatives: **[[Optimizers|Muon]]** (orthogonalized momentum — ~2× compute efficiency vs AdamW at compute-optimal scale, [[Muon is Scalable for LLM Training (2025)|Liu 2025]], with scale-dependent caveats per [[Fantastic Pretraining Optimizers II - Hyperball (2026)|Wen 2026]]) and second-order-lite methods (Shampoo/SOAP) — the first serious challenge to Adam's decade of dominance; full family map in [[Optimizers]]

## Related

- Adaptive extension of [[Gradient Descent]] (momentum = $\beta_1$ half)
- Warmup connects to [[Normalization]] placement; hyperparameter transfer to [[Weight Initialization]]/μP
- The z-loss family ([[ST-MoE (2022)]], [[Unembedding]]) exists because Adam-trained bf16 logits misbehave

## Sources

- [[Adam - A Method for Stochastic Optimization (2014)]] — the algorithm
- [[Decoupled Weight Decay Regularization - AdamW (2017)]] — the fix + cosine schedules
- [[On Layer Normalization in the Transformer Architecture (2020)]] — warmup's architectural origin
- [[Tensor Programs V - muTransfer (2022)]] — LR transfer across scale

---
Part of the foundations cluster.
