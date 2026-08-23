---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [Nonlinearity, Activation]
---

# Activation Function

The element-wise non-linear function $\sigma(\cdot)$ applied after a layer's linear transform. It is the *only* source of non-linearity in a [[Neural Network]] — without it, any stack of layers collapses into a single [[Matrix Multiplication]], and depth buys nothing. The choice of activation shapes gradient flow through [[Backpropagation]], and therefore how trainable a deep network is.

![[activation-functions.png]]

## ⚡ Quick summary — who uses what, and why

| Activation | Introduced by | Flagship adopters | Why they chose it |
| --- | --- | --- | --- |
| ReLU | Nair & Hinton 2010 (see [[Deep Learning using Rectified Linear Units (2018)\|attribution]]) | **AlexNet, VGG, ResNet, Inception**; edge/mobile | Non-saturating gradient at zero cost; hardware-trivial |
| Leaky ReLU | Maas et al. 2013; eval. [[Empirical Evaluation of Rectified Activations in Convolutional Network (2015)\|Xu 2015]] | **DCGAN discriminators, YOLO/Darknet** | Dead neurons are fatal in GAN training; near-zero extra cost |
| PReLU | He et al. 2015 | **ResNet-era ImageNet winners** | Learned slope; first above-human top-5 ImageNet |
| ELU | [[Fast and Accurate Deep Network Learning by Exponential Linear Units (2015)\|Clevert 2015]] | niche CNNs; descendant SELU | Zero-mean push ≈ free normalization pre-batch-norm |
| GELU | [[Gaussian Error Linear Units (2016)\|Hendrycks & Gimpel 2016]] | **BERT, GPT-1/2/3, ViT, Swin** | Smooth gradient flow at transformer depth; wins across modalities |
| Swish / h-swish | [[Searching for Activation Functions (2017)\|Ramachandran 2017]] | **EfficientNet, MobileNetV3** (h-swish) | Search-validated gains on deep CNNs; h-swish = cheap mobile approx. |
| Mish | [[Mish - A Self Regularized Non-Monotonic Activation Function (2020)\|Misra 2020]] | **YOLOv4/v5-era detectors** | Best-in-class COCO detection gains |
| SwiGLU / GEGLU | [[GLU Variants Improve Transformer (2020)\|Shazeer 2020]] | **PaLM, LLaMA 1–3, Mistral, Qwen, Gemma** | Free perplexity win at fixed params; faster convergence ([[The Devil is in the Condition Numbers (2026)\|NTK view]]) |

### Which is better for which setting

Per the cross-benchmark survey ([[Activation Functions in Deep Learning - A Comprehensive Survey and Benchmark (2021)|Dubey 2021]]), **no activation wins everywhere** — the choice interacts with architecture, depth, and scale:

- **Small CNNs / small datasets** → **ReLU**. Smooth variants show no reliable gain here ([[Benchmarking Comparison of Swish vs Other Activation Functions on CIFAR-10 (2019)|Szandala 2019]]) and cost more.
- **Deep vision CNNs at scale** (ImageNet+) → **Swish or Mish**; gains grow with depth.
- **Object detection** → **Mish** (COCO evidence) or Leaky ReLU for speed.
- **GANs** → **Leaky ReLU** in discriminators — dying neurons are catastrophic there.
- **Transformers (encoder / vision)** → **GELU** — the BERT/ViT standard.
- **LLM-scale transformers** → **SwiGLU FFN** — the current default, effectively free.
- **Mobile / edge / FPGA** → **ReLU or h-swish** — transcendentals (erf, exp) are the enemy of cheap hardware.
- **Output layers** are a different question: sigmoid/softmax for probabilities, linear for regression — the saturating functions live on here.

**The arc in one line:** bounded & smooth → unbounded & cheap (ReLU) → smooth again but non-saturating (GELU/Swish) → gated and learned (SwiGLU).

---

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

**The fatal flaw:** both *saturate* — for large $|x|$ the derivative is $\approx 0$. In a deep network the chain-rule product of many near-zero derivatives → **vanishing gradients**; early layers stop learning. Empirically, sigmoid networks fail to converge at all in deep convolutional tasks, performing at chance level ([[Deep Learning using Rectified Linear Units (2018)|Agarap 2018, rev.]]).

**Verdict:** dead for hidden layers; sigmoid survives only as a *gate* (attention scores, LSTM gates, and inside Swish/GLU below).

---

## ReLU — the workhorse

$$\text{ReLU}(x) = \max(0, x)$$

Introduced into deep learning by **Nair & Hinton (2010)** — a frequently mis-cited origin that the literature had to formally correct ([[Deep Learning using Rectified Linear Units (2018)]]).

**Motivation:** kill vanishing gradients. For $x>0$ the derivative is exactly 1 — the identity — so gradients pass through any number of layers undiminished.

**Intuition:** each neuron is a switch: on (linear) or off (zero). A ReLU network is a piecewise-linear function — it carves input space into an enormous number of linear regions, and depth grows that number super-exponentially: there exist functions a deep net represents compactly that any shallow net needs $\sim \frac{1}{2}k^{k+1}$ units to match ([[Understanding Deep Neural Networks with Rectified Linear Units (2016)|Arora et al. 2016]]). Spline theory additionally shows ReLU solutions are the *natural* piecewise-linear splines of regularized network training — a principled optimum, not a hack ([[The Role of Neural Network Activation Functions (2019)|Parhi & Nowak 2019]]).

**Results & adoption:** the enabler of AlexNet (2012) and the entire CNN era — VGG, ResNet, Inception all default to ReLU. Cheapest possible non-linearity, and still the hardware-friendly choice for edge deployment.

**The flaw — dying ReLU:** a neuron pushed into the negative region for all inputs has zero gradient *forever* and never recovers; the death mechanism is initialization-dependent and formally analyzed in [[ReLU Neural Networks and Their Training (2025)|Luo et al. 2025]].

## The ReLU repair family: Leaky ReLU, PReLU, ELU

**Motivation:** keep ReLU's non-saturating positive side, fix the dead negative side.

- **Leaky ReLU** ($\max(\alpha x, x)$, fixed small $\alpha$) and **PReLU** (learned $\alpha$): the systematic evaluation in [[Empirical Evaluation of Rectified Activations in Convolutional Network (2015)|Xu et al. 2015]] found *any* non-zero negative slope consistently beats plain ReLU — evidence **against** the belief that ReLU's sparsity is what makes it good. Their randomized RReLU generalized best on small data (75.68% CIFAR-100). Adopted by: **GAN discriminators (DCGAN), YOLO/Darknet backbones** (Leaky); PReLU powered the first above-human-level ImageNet top-5 result (He et al. 2015).
- **ELU** ($x$ / $\alpha(e^x{-}1)$): pushes mean activations toward zero — batch norm's centering effect for free — and its negative saturation gives a noise-robust off-state. Results: faster learning and better generalization than ReLU on nets ≥ 5 layers; on CIFAR-100, ELU nets **beat ReLU + batch norm** ([[Fast and Accurate Deep Network Learning by Exponential Linear Units (2015)|Clevert et al. 2015]]).

**Verdict:** real but modest gains; none displaced ReLU — too inconsistent to pay the switching cost, per the field-wide benchmark [[Activation Functions in Deep Learning - A Comprehensive Survey and Benchmark (2021)|Dubey et al. 2021]].

---

## The smooth self-gated era: GELU & Swish

The modern default family. Common form: $x \cdot g(x)$ — the input multiplied by a soft gate of itself, instead of hard-switched by its sign.

### GELU

$$\text{GELU}(x) = x \cdot \Phi(x)$$

where $\Phi$ is the standard Gaussian CDF.

**Motivation/intuition:** the beautiful one — GELU is the *expected value of a stochastic regularizer* that multiplies each input by 0 or 1 with probability $\Phi(x)$: dropout whose keep-probability depends on the input's value. Averaging that noise gives a smooth, **non-monotonic** curve (dips to ≈ −0.17 near $x \approx -0.75$) that weights inputs by value rather than gating by sign ([[Gaussian Error Linear Units (2016)|Hendrycks & Gimpel 2016]]). An equivalent modern view: a hard gate with a Gaussian-random threshold ([[A Structural Interpretation of GELU (2026)|Rossi 2026]]).

**Results:** beat ReLU and ELU across every task tested — vision, NLP, speech ([[Gaussian Error Linear Units (2016)]]); independently replicated on residual CNNs seven years later ([[Mathematical Analysis and Performance Evaluation of GELU (2023)|Lee 2023]]).

### Swish / SiLU

$$\text{Swish}(x) = x \cdot \text{sigmoid}(\beta x)$$

**Motivation:** found by *automated search* (exhaustive + RL) — and the search independently converged on essentially GELU's shape, strong evidence that smooth-self-gated is a real optimum ([[Searching for Activation Functions (2017)|Ramachandran et al. 2017]]). With $\beta=1$ it's SiLU. Both belong to one "threshold-transmission" family with ReLU and hard-swish, differing only in the gate's noise distribution ([[A Structural Interpretation of GELU (2026)]]).

**Results:** +0.9% ImageNet top-1 on Mobile NASNet-A, +0.6% on Inception-ResNet-v2, gains growing with depth ([[Searching for Activation Functions (2017)]]). **Caveat:** an independent small-scale replication found *no* gain over ReLU on CIFAR-10 ([[Benchmarking Comparison of Swish vs Other Activation Functions on CIFAR-10 (2019)|Szandala 2019]]) — the advantage is a deep-and-large-scale phenomenon.

### Mish

$$\text{Mish}(x) = x \tanh(\text{softplus}(x))$$

Same family, slightly wider negative dip, argued to self-regularize the loss landscape. ≈ +1% ImageNet top-1 over ReLU on ResNet-50; +2.1% AP₅₀ on YOLOv4/MS-COCO over Leaky ReLU ([[Mish - A Self Regularized Non-Monotonic Activation Function (2020)|Misra 2020]]).

**Why smooth + non-monotonic wins (intuition):** no dead zone (gradient exists everywhere), the slight negative dip adds expressivity, and smoothness stabilizes optimization — collectively worth ~0.5–1% accuracy, decisive at scale.

---

## The gated era: GLU variants & SwiGLU

Not a drop-in activation but a redesign of the [[Feedforward Network]] layer itself — see [[GLU Variants]].

$$\text{SwiGLU}(x) = (W_1 x) \otimes \text{Swish}(W_2 x)$$

**Motivation:** instead of each scalar gating itself, compute *two* linear projections and let one gate the other — a learned, content-dependent gate ([[GLU Variants Improve Transformer (2020)|Shazeer 2020]]).

**Results:** at fixed parameter count, GEGLU/SwiGLU beat both ReLU and GELU transformer FFNs on pre-training perplexity and GLUE/SuperGLUE — offered famously with "no explanation… other than divine benevolence." The explanation arrived later: gating reshapes the NTK spectrum (smaller condition number → **faster optimization**, not better generalization) ([[The Devil is in the Condition Numbers (2026)|Lyu et al. 2026]]).

---

## Related

- The source of non-linearity in every [[Neural Network]]
- Lives inside the [[Feedforward Network]] block of the [[Transformer]]
- SwiGLU belongs to [[GLU Variants]] — an FFN redesign, not an element-wise activation
- Derivative behavior directly determines [[Backpropagation]] gradient flow (vanishing/dying gradients)
- Sigmoid survives as the *gate* in attention scores and GLU-style gating

## Sources

- [[Deep Learning using Rectified Linear Units (2018)]] — ReLU attribution + sigmoid-collapse evidence
- [[Understanding Deep Neural Networks with Rectified Linear Units (2016)]] — depth-separation theory
- [[The Role of Neural Network Activation Functions (2019)]] — spline-theoretic justification of ReLU
- [[ReLU Neural Networks and Their Training (2025)]] — dying-ReLU mechanism
- [[Empirical Evaluation of Rectified Activations in Convolutional Network (2015)]] — Leaky/PReLU/RReLU; anti-sparsity finding
- [[Fast and Accurate Deep Network Learning by Exponential Linear Units (2015)]] — ELU
- [[Gaussian Error Linear Units (2016)]] — GELU
- [[Mathematical Analysis and Performance Evaluation of GELU (2023)]] — independent GELU replication
- [[A Structural Interpretation of GELU (2026)]] — threshold-transmission unification
- [[Searching for Activation Functions (2017)]] — Swish
- [[Benchmarking Comparison of Swish vs Other Activation Functions on CIFAR-10 (2019)]] — failed Swish replication
- [[Mish - A Self Regularized Non-Monotonic Activation Function (2020)]] — Mish
- [[GLU Variants Improve Transformer (2020)]] — SwiGLU
- [[The Devil is in the Condition Numbers (2026)]] — why GLU wins (NTK)
- [[Activation Functions in Deep Learning - A Comprehensive Survey and Benchmark (2021)]] — the neutral referee

---
Part of the [[Transformer]] cluster
