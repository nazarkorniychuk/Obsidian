---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [Nonlinearity, Activation]
---

# Activation Function

The element-wise non-linear function $\sigma(\cdot)$ applied after a layer's linear transform. It is the *only* source of non-linearity in a [[Neural Network]] — without it, any stack of layers collapses into a single [[Matrix Multiplication]], and depth buys nothing. The choice of activation shapes gradient flow through [[Backpropagation]], and therefore how trainable a deep network is.

## The design tensions

Every activation is a compromise between:

1. **Gradient flow** — the derivative shouldn't vanish (saturating functions) or die permanently (hard zero regions)
2. **Non-linearity** — enough expressive power to bend the function
3. **Compute cost** — evaluated billions of times; `max(0,x)` vs `erf` matters
4. **Optimization landscape** — smoothness, zero-centered outputs, and self-regularization all affect convergence speed

The history of activations is the history of rebalancing these four.

---

## The saturating era: sigmoid & tanh

$$\text{sigmoid}(x) = \frac{1}{1+e^{-x}} \qquad \tanh(x)$$

**Motivation:** biological plausibility (firing rate between bounds) and smooth differentiability, back when networks were shallow.

**The fatal flaw:** both *saturate* — for large $|x|$ the derivative is $\approx 0$. In a deep network the chain-rule product of many near-zero derivatives → **vanishing gradients**; early layers stop learning. Empirically, sigmoid networks fail to converge at all in deep convolutional tasks, performing at chance level [1]. Tanh survives longer (it's zero-centered) but saturates just the same.

**Verdict:** dead for hidden layers; sigmoid survives only as a *gate* (attention scores, LSTM gates, and inside Swish/GLU below).

---

## ReLU — the workhorse

$$\text{ReLU}(x) = \max(0, x)$$

Introduced into deep learning by Nair & Hinton (2010) — a frequently mis-cited origin that the literature has had to formally correct [1].

**Motivation:** kill vanishing gradients. For $x>0$ the derivative is exactly 1 — the identity — so gradients pass through any number of layers undiminished.

**Intuition:** each neuron is a switch: on (linear) or off (zero). A ReLU network is a piecewise-linear function — it carves input space into an enormous number of linear regions, and depth grows that number super-exponentially: there exist functions a $k^2$-layer ReLU net represents with $k^3$ units that any $k$-layer net needs $\sim \frac{1}{2}k^{k+1}$ units to match [2]. Theory also confirms ReLU nets are universal approximators and explains the popularity via spline-theoretic optimality [3].

**Results:** enabler of the 2012 deep-learning breakthrough (AlexNet); still the default in many CNNs. Cheapest possible non-linearity.

**The flaw — dying ReLU:** a neuron pushed into the negative region for all inputs has zero gradient *forever* and never recovers; the neuron-death mechanism has been analyzed formally [4].

## The ReLU repair family: Leaky ReLU, PReLU, ELU

**Motivation:** keep ReLU's non-saturating positive side, fix the dead negative side.

- **Leaky ReLU** — small fixed slope $\alpha x$ for $x<0$. **PReLU** — learn $\alpha$. A systematic evaluation found that *any* non-zero negative slope consistently beats plain ReLU — evidence *against* the belief that ReLU's sparsity is what makes it good [5].
- **ELU** — $\alpha(e^x - 1)$ for $x<0$: smooth, saturates to $-\alpha$. **Motivation:** push mean activations toward zero (a cheap substitute for batch norm's centering effect, reducing "bias shift"), while the negative saturation makes the off-state noise-robust. **Results:** faster learning and better generalization than ReLU/Leaky ReLU on networks ≥5 layers; on CIFAR-100, ELU nets beat ReLU+batch-norm nets [6].

**Verdict:** real but modest gains; none displaced ReLU in practice — the gains were too inconsistent to pay the switching cost [7].

---

## The smooth self-gated era: GELU & Swish

The modern default family. Common form: $x \cdot g(x)$ — the input multiplied by a soft gate of itself, instead of hard-switched by its sign.

### GELU

$$\text{GELU}(x) = x \cdot \Phi(x)$$

where $\Phi$ is the standard Gaussian CDF [8].

**Motivation/intuition:** the beautiful one — GELU is the *expected value of a stochastic regularizer* that randomly multiplies each input by 0 or 1 with probability $\Phi(x)$: dropout-like noise whose keep-probability depends on the input's value. Deterministically averaging that noise gives a smooth, **non-monotonic** curve (it dips slightly below zero around $x\approx-0.6$) that weights inputs by value rather than gating by sign [8, 9]. Equivalently: a hard gate with a Gaussian-random threshold [10].

**Results:** outperformed ReLU and ELU across vision, NLP, and speech tasks in the original evaluation [8], with later independent analysis confirming its edge on CIFAR-10/100 and STL-10 [11]. **Adopted by BERT, GPT-2/3, ViT** — the transformer-era default.

### Swish / SiLU

$$\text{Swish}(x) = x \cdot \text{sigmoid}(\beta x)$$

**Motivation:** found by *automated search* (exhaustive + RL) over a space of candidate functions — the search independently converged on the same shape as GELU [12]. With $\beta=1$ it's called SiLU; GELU and SiLU are near-identical curves (both are members of one "threshold-transmission" family [10]).

**Results:** replacing ReLU with Swish gave +0.9% top-1 on ImageNet for Mobile NASNet-A and +0.6% for Inception-ResNet-v2, with gains growing on deeper models [12, 13]. Caveat: gains are not universal — some replications on smaller settings found no improvement over ReLU [14].

**Why smooth+non-monotonic wins (intuition):** no dead zone (gradient exists everywhere), the slight negative dip adds expressivity, and smoothness stabilizes gradient flow near zero — collectively worth ~0.5–1% accuracy, which at modern scale is decisive. Followers in this family: **Mish**, $x\tanh(\text{softplus}(x))$, which beat both ReLU and Swish on ImageNet and COCO detection [15].

---

## The gated era: GLU variants & SwiGLU

Not a drop-in activation but a redesign of the [[Feedforward Network]] layer itself — see [[GLU Variants]].

$$\text{GLU}(x) = (W_1 x) \otimes \sigma(W_2 x) \qquad \text{SwiGLU}(x) = (W_1 x) \otimes \text{Swish}(W_2 x)$$

**Motivation:** instead of gating each scalar by itself, compute *two* linear projections and let one gate the other — a learned, content-dependent gate [16].

**Results:** Shazeer tested GLU variants in the transformer FFN and found consistent quality improvements over both ReLU and GELU, with SwiGLU/GEGLU the best [16] — famously offering "no explanation… other than divine benevolence." A proposed actual explanation: gating reshapes the NTK spectrum (smaller condition number → faster optimization), i.e. the benefit is *trainability*, not generalization [17].

**Adopted by LLaMA, PaLM, Mistral, Qwen — SwiGLU is the current LLM default.** Note the shift: from gating *within* a token's scalar to mixing between two projections — the activation stopped being element-wise self-contained.

---

## Cheat sheet

| Activation | Formula | Killer feature | Fatal flaw | Used in |
|---|---|---|---|---|
| sigmoid/tanh | $\frac{1}{1+e^{-x}}$ | smooth, bounded | vanishing gradients [1] | gates only |
| ReLU | $\max(0,x)$ | cheap, gradient=1 | dying neurons [4] | CNNs |
| Leaky/PReLU | $\max(\alpha x, x)$ | no dead zone [5] | marginal gains | some CNNs |
| ELU | $x$ / $\alpha(e^x{-}1)$ | zero-mean push [6] | exp cost | niche |
| GELU | $x\Phi(x)$ | smooth, stochastic interp. [8] | erf cost | BERT, GPT, ViT |
| Swish/SiLU | $x\,\sigma(\beta x)$ | search-found, ≈GELU [12] | sigmoid cost | EfficientNet |
| SwiGLU | $(W_1x)\otimes\text{Swish}(W_2x)$ | best LLM quality [16] | 3 matrices not 2 | LLaMA, PaLM, Mistral |

**The arc in one line:** bounded & smooth → unbounded & cheap (ReLU) → smooth again but non-saturating (GELU/Swish) → gated and learned (SwiGLU).

## Related

- The source of non-linearity in every [[Neural Network]]
- Lives inside the [[Feedforward Network]] block of the [[Transformer]]
- SwiGLU belongs to [[GLU Variants]] — an FFN redesign, not an element-wise activation
- Derivative behavior directly determines [[Backpropagation]] gradient flow (vanishing/dying gradients)
- Sigmoid survives as the *gate* in attention scores and GLU-style gating

## References

1. [Deep Learning using Rectified Linear Units (ReLU)](https://consensus.app/papers/details/b9cb4589c00257e38a1ad9dfb68688f5/?utm_source=claude_desktop) — Agarap (rev. w/ historical correction crediting Nair & Hinton 2010), DOI: 10.48550/arxiv.1803.08375
2. [Understanding Deep Neural Networks with Rectified Linear Units](https://consensus.app/papers/details/4a32bf9d9f235821be012619e1a305f0/?utm_source=claude_desktop) — Arora et al., 2016, DOI: 10.48550/arxiv.1611.01491
3. [The Role of Neural Network Activation Functions](https://consensus.app/papers/details/956924c9cc3a5e0b8aac84222daccf4e/?utm_source=claude_desktop) — Parhi & Nowak, 2019, DOI: 10.1109/lsp.2020.3027517
4. [ReLU Neural Networks and Their Training](https://consensus.app/papers/details/bb0f8d2bab9254159b215b36d638c908/?utm_source=claude_desktop) — Luo et al., 2025, DOI: 10.3390/math14010039
5. [Empirical Evaluation of Rectified Activations in Convolutional Network](https://consensus.app/papers/details/1e20b8cfa3c15b1883a28a7d4a322104/?utm_source=claude_desktop) — Xu et al., 2015, DOI: 10.48550/arxiv.1505.00853
6. [Fast and Accurate Deep Network Learning by Exponential Linear Units (ELUs)](https://consensus.app/papers/details/046aee25d6995601b4bb4d619c97fa2d/?utm_source=claude_desktop) — Clevert, Unterthiner & Hochreiter, 2015, DOI: 10.48550/arxiv.1511.07289
7. [Activation functions in deep learning: A comprehensive survey and benchmark](https://consensus.app/papers/details/adb6f177ec36516981daac7d3e9ea3d5/?utm_source=claude_desktop) — Dubey et al., 2021, DOI: 10.1016/j.neucom.2022.06.111
8. [Gaussian Error Linear Units (GELUs)](https://consensus.app/papers/details/fc847c9b3707508aae7f2859f66a47a9/?utm_source=claude_desktop) — Hendrycks & Gimpel, 2016, DOI: 10.48550/arxiv.1606.08415
9. [Bridging Nonlinearities and Stochastic Regularizers with Gaussian Error Linear Units](https://consensus.app/papers/details/f3478062c8535adaa13d3c9bc0095a21/?utm_source=claude_desktop) — Hendrycks & Gimpel, 2016
10. [A Structural Interpretation of GELU and Threshold-Transmission Activations](https://consensus.app/papers/details/b71a5bf56ad45e55b4b95c2e4175a06b/?utm_source=claude_desktop) — Rossi, 2026, DOI: 10.48550/arxiv.2607.03664
11. [Mathematical Analysis and Performance Evaluation of the GELU Activation Function in Deep Learning](https://consensus.app/papers/details/c79409ece0f25b02a0a93a74be510eae/?utm_source=claude_desktop) — Lee, 2023, DOI: 10.1155/2023/4229924
12. [Searching for Activation Functions](https://consensus.app/papers/details/0893b159d5875de2a7e4467b88054675/?utm_source=claude_desktop) — Ramachandran, Zoph & Le, 2017, DOI: 10.48550/arxiv.1710.05941
13. [Swish: a Self-Gated Activation Function](https://consensus.app/papers/details/e584e1066bee56d68303e85d852854f6/?utm_source=claude_desktop) — Ramachandran et al., 2017, DOI: 10.48550/arxiv.1710.05941
14. [Benchmarking Comparison of Swish vs. Other Activation Functions on CIFAR-10](https://consensus.app/papers/details/af57fe91e93c5f3a91ae52857f22a5ad/?utm_source=claude_desktop) — Szandala, 2019, DOI: 10.1007/978-3-030-19501-4_49
15. [Mish: A Self Regularized Non-Monotonic Activation Function](https://consensus.app/papers/details/45b489ac1e2a5e44a7591c1e932bcc1e/?utm_source=claude_desktop) — Misra, 2020, DOI: 10.5244/c.34.191
16. [GLU Variants Improve Transformer](https://consensus.app/papers/details/a61336c71bd8535aa0f6696f4a7a7bda/?utm_source=claude_desktop) — Shazeer, 2020, DOI: 10.48550/arxiv.2002.05202
17. [The Devil is in the Condition Numbers: Why is GLU Better than non-GLU Structure?](https://consensus.app/papers/details/7eb498afe2f45d0aa3f490928875e399/?utm_source=claude_desktop) — Lyu et al., 2026, DOI: 10.48550/arxiv.2605.20749

---
Part of the [[Transformer]] cluster
