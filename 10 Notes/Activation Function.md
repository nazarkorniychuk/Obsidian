---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [Nonlinearity, Activation]
---

# Activation Function

Element-wise non-linear function $\sigma(\cdot)$ after a layer's linear transform. The only source of non-linearity in a [[Neural Network]] — without it, stacked layers collapse into one [[Matrix Multiplication]]. Its derivative determines gradient flow through [[Backpropagation]]: saturation → vanishing gradients; hard zero regions → dead neurons.

![[activation-functions.png]]

## ⚡ Adoption table

| Activation | Introduced by | Flagship adopters | Reason |
| --- | --- | --- | --- |
| ReLU | Nair & Hinton 2010 (see [[Deep Learning using Rectified Linear Units (2018)\|attribution]]) | **AlexNet, VGG, ResNet, Inception**; edge/mobile | Non-saturating gradient at zero cost; hardware-trivial |
| Leaky ReLU | Maas et al. 2013; eval. [[Empirical Evaluation of Rectified Activations in Convolutional Network (2015)\|Xu 2015]] | **DCGAN discriminators, YOLO/Darknet** | Dead neurons fatal in GAN training; near-zero extra cost |
| PReLU | He et al. 2015 | **ResNet-era ImageNet winners** | Learned slope; first above-human top-5 ImageNet |
| ELU | [[Fast and Accurate Deep Network Learning by Exponential Linear Units (2015)\|Clevert 2015]] | niche CNNs; descendant SELU | Zero-mean push ≈ free normalization pre-batch-norm |
| GELU | [[Gaussian Error Linear Units (2016)\|Hendrycks & Gimpel 2016]] | **BERT, GPT-1/2/3, ViT, Swin** | Wins across modalities at transformer depth |
| Swish / h-swish | [[Searching for Activation Functions (2017)\|Ramachandran 2017]] | **EfficientNet, MobileNetV3** (h-swish) | Search-validated gains on deep CNNs; h-swish = cheap mobile approx. |
| Mish | [[Mish - A Self Regularized Non-Monotonic Activation Function (2020)\|Misra 2020]] | **YOLOv4/v5-era detectors** | Best-in-class COCO detection gains |
| SwiGLU / GEGLU | [[GLU Variants Improve Transformer (2020)\|Shazeer 2020]] | **PaLM, LLaMA 1–3, Mistral, Qwen, Gemma** | Perplexity win at fixed params; faster convergence ([[The Devil is in the Condition Numbers (2026)\|NTK view]]) |

## ⚡ Which for which setting

No activation wins everywhere; choice interacts with architecture, depth, scale ([[Activation Functions in Deep Learning - A Comprehensive Survey and Benchmark (2021)|Dubey 2021]], 18-activation benchmark):

- **Small CNNs / small data** → **ReLU** — smooth variants show no reliable gain ([[Benchmarking Comparison of Swish vs Other Activation Functions on CIFAR-10 (2019)|Szandala 2019]]) and cost more
- **Deep vision CNNs at scale** → **Swish or Mish** — gains grow with depth
- **Object detection** → **Mish** (COCO evidence) or Leaky ReLU for speed
- **GANs** → **Leaky ReLU** in discriminators
- **Transformers (encoder/vision)** → **GELU**
- **LLM-scale transformers** → **SwiGLU FFN**
- **Mobile / edge / FPGA** → **ReLU or h-swish** — no transcendentals
- **Output layers** → sigmoid/softmax (probabilities), linear (regression)

**Arc:** bounded & smooth → unbounded & cheap (ReLU) → smooth non-saturating (GELU/Swish) → gated & learned (SwiGLU).

---

## sigmoid, tanh

$$\text{sigmoid}(x) = \tfrac{1}{1+e^{-x}} \qquad \tanh(x)$$

- Saturate: derivative → 0 for large $|x|$; chain-rule product across layers → **vanishing gradients**
- Result: sigmoid nets **fail to converge** (chance-level accuracy) in deep conv tasks; tanh converges but loses to ReLU on classification, wins only on bounded-output tasks like image reconstruction ([[Deep Learning using Rectified Linear Units (2018)|Agarap 2018 rev.]], 10-trial Kruskal-Wallis)
- **Conclusion:** dead as hidden activations; sigmoid survives as gate (LSTM, attention, Swish/GLU)

## ReLU

$$\text{ReLU}(x) = \max(0, x)$$

- Origin: Nair & Hinton 2010 — widely mis-cited; record corrected in [[Deep Learning using Rectified Linear Units (2018)]]
- Derivative = 1 for $x>0$: gradients pass through arbitrary depth undiminished → enabled AlexNet 2012 and the CNN era
- **Expressivity results:** depth separation is super-exponential — functions computable at size $k^3$ with $k^2$ layers require $\sim \frac{1}{2}k^{k+1}$ nodes at $k$ layers ([[Understanding Deep Neural Networks with Rectified Linear Units (2016)|Arora 2016]]); ReLU solutions coincide with optimal piecewise-linear splines of regularized training, same framework justifies weight decay and skip connections ([[The Role of Neural Network Activation Functions (2019)|Parhi & Nowak 2019]])
- **Failure mode:** dying ReLU — neuron negative on the whole data distribution has zero gradient permanently; initialization-dependent, formalized in [[ReLU Neural Networks and Their Training (2025)|Luo 2025]]
- **Conclusion:** still optimal where compute cost dominates or nets are shallow

## Leaky ReLU, PReLU, RReLU

- $\max(\alpha x, x)$; $\alpha$ fixed (Leaky), learned (PReLU), randomized in training (RReLU)
- **Results** ([[Empirical Evaluation of Rectified Activations in Convolutional Network (2015)|Xu 2015]], CIFAR-10/100): any non-zero negative slope **consistently beats plain ReLU** → refutes the sparsity-is-the-key hypothesis; Leaky/PReLU overfit on small data, RReLU best there (75.68% CIFAR-100 no ensemble)
- PReLU: component of first above-human ImageNet top-5 (He 2015)
- **Conclusion:** the negative-slope fix is real but small; adoption limited to niches where dead neurons are catastrophic (GANs, detection backbones)

## ELU

$$\text{ELU}(x) = x \;(x>0), \quad \alpha(e^x - 1) \;(x \le 0)$$

- Negative outputs push mean activations → 0 (reduces bias shift ≈ batch-norm centering for free); negative saturation at $-\alpha$ gives noise-robust off-state
- **Results** ([[Fast and Accurate Deep Network Learning by Exponential Linear Units (2015)|Clevert 2015]]): faster learning + better generalization than ReLU/Leaky on nets ≥ 5 layers; **CIFAR-100: ELU beats ReLU + batch norm**, and batch norm adds nothing on top of ELU; best published CIFAR-100 at the time; ImageNet training speedup at equal architecture
- **Conclusion:** never displaced ReLU — exp cost + gains shrank once batch norm standard; per [[Activation Functions in Deep Learning - A Comprehensive Survey and Benchmark (2021)|Dubey 2021]] the repair family's gains are too inconsistent to pay switching cost

## GELU

$$\text{GELU}(x) = x \cdot \Phi(x)$$

- = expectation of Bernoulli($\Phi(x)$) input gating — input-adaptive dropout averaged; equivalently a hard gate with Gaussian-random threshold ([[A Structural Interpretation of GELU (2026)|Rossi 2026]])
- Smooth, non-monotonic (min ≈ −0.17 at $x \approx -0.75$), gradient everywhere — no dead neurons
- **Results:** beat ReLU and ELU on **every task tested** — MNIST/CIFAR vision, POS tagging, TIMIT speech ([[Gaussian Error Linear Units (2016)]]); independently confirmed on residual CNNs, CIFAR-10/100 + STL-10 ([[Mathematical Analysis and Performance Evaluation of GELU (2023)|Lee 2023]])
- **Conclusion:** the transformer-era default (BERT, GPT, ViT); superseded only by gated FFNs at LLM scale

## Swish / SiLU

$$\text{Swish}(x) = x \cdot \text{sigmoid}(\beta x)$$

- Found by automated search (exhaustive + RL); search converged on ≈ GELU's shape — evidence the smooth-self-gated shape is a genuine optimum, not a guess ([[Searching for Activation Functions (2017)|Ramachandran 2017]]); ReLU/GELU/SiLU/hard-swish form one threshold-transmission family differing only in gate-noise distribution ([[A Structural Interpretation of GELU (2026)]])
- **Results:** ImageNet top-1 **+0.9%** (Mobile NASNet-A), **+0.6%** (Inception-ResNet-v2) over ReLU as drop-in; gains grow with depth
- **Counter-result:** no gain over ReLU on small CNN / CIFAR-10 ([[Benchmarking Comparison of Swish vs Other Activation Functions on CIFAR-10 (2019)|Szandala 2019]]) → advantage is a depth-and-scale phenomenon
- **Conclusion:** adopted where depth is high and compute matters less (EfficientNet); h-swish approximation for mobile (MobileNetV3)

## Mish

$$\text{Mish}(x) = x \tanh(\text{softplus}(x))$$

- Swish family; wider negative dip; first-derivative shape argued to smooth the loss landscape
- **Results** ([[Mish - A Self Regularized Non-Monotonic Activation Function (2020)|Misra 2020]]): **ImageNet ≈ +1%** top-1 over ReLU (ResNet-50, identical hyperparams); **MS-COCO +2.1% AP₅₀** over Leaky ReLU (YOLOv4/CSP-DarkNet-53); consistent wins over ReLU *and* Swish in benchmark sweeps
- **Conclusion:** detection niche champion; never entered transformers (GELU/SwiGLU had won; tanh+softplus costlier)

## SwiGLU / GLU variants

$$\text{SwiGLU}(x) = (W_1 x) \otimes \text{Swish}(W_2 x)$$

- FFN redesign, not element-wise: two projections, one gates the other — see [[GLU Variants]]; tested variants ReGLU/GEGLU/SwiGLU at fixed parameter count ($d_{ff}$ shrunk 2/3)
- **Results** ([[GLU Variants Improve Transformer (2020)|Shazeer 2020]]): GEGLU/SwiGLU **beat ReLU and GELU FFNs** on T5 pre-training perplexity and GLUE/SuperGLUE fine-tuning; offered with "no explanation… other than divine benevolence"
- **Mechanism** ([[The Devil is in the Condition Numbers (2026)|Lyu 2026]], NTK regime): gating shrinks NTK condition number → **faster optimization**; effect on generalization gap ≈ nil (ViT, GPT-2) — the win is trainability at fixed compute
- **Conclusion:** de-facto LLM FFN standard (PaLM, LLaMA, Mistral, Qwen, Gemma)

---

## Related

- Source of non-linearity in every [[Neural Network]]
- Lives inside the [[Feedforward Network]] block of the [[Transformer]]
- SwiGLU belongs to [[GLU Variants]] — an FFN redesign, not an element-wise activation
- Derivative behavior determines [[Backpropagation]] gradient flow (vanishing/dying gradients)
- Sigmoid survives as the gate in attention scores and GLU-style gating

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
