---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [NN, feedforward network, MLP, deep learning model]
---

# Neural Network

A function built by composing linear maps with element-wise non-linearities:

$$h^{(l+1)} = \sigma\big(W^{(l)} h^{(l)} + b^{(l)}\big)$$

Each layer re-represents its input; depth = repeated composition. Without the [[Activation Function]] $\sigma$, the stack collapses into one [[Matrix Multiplication]] — the non-linearity is what depth buys expressiveness with.

## Expressivity — what the architecture *can* represent

- **Universal approximation:** one hidden layer with any non-polynomial activation approximates any (Borel-measurable) function to arbitrary accuracy, given enough units ([[Multilayer Feedforward Networks are Universal Approximators (1989)|Hornik 1989]]). So representational capacity is never the bottleneck — failures are optimization or generalization failures
- **What the theorem doesn't give:** the unit count (can be exponential), learnability, or generalization. **Depth changes the economics:** functions exist that deep ReLU nets represent at polynomial size while any shallow net needs super-exponentially many units ($\sim \frac{1}{2}k^{k+1}$) ([[Understanding Deep Neural Networks with Rectified Linear Units (2016)|Arora 2016]]) — the formal case for deep over wide

## Training — the three-piece loop

1. **Loss** measuring wrongness → 2. **[[Backpropagation]]** computing all parameter gradients at ~2–3× forward cost → 3. **[[Gradient Descent]]/[[Adam Optimizer]]** updating weights — started from a variance-preserving [[Weight Initialization]], which historically was the difference between "untrainable" and "state of the art" ([[On the Importance of Initialization and Momentum (2013)|Sutskever 2013]]). The idealized-limit theory of this loop is the [[Neural Tangent Kernel]].

## Generalization — the classical puzzle and the modern picture

- **Classical view:** bias-variance tradeoff; more parameters → overfitting → regularize. Canonical tool: [[Dropout (2014)|dropout]] (random unit zeroing ≈ training an exponential ensemble; ~44k citations of usefulness in the small-data era)
- **The modern violation:** heavily over-parameterized networks that can memorize noise still generalize; test error can *re-descend* past the interpolation point (**double descent**, characterized exactly in random-features models — [[The Generalization Error of Random Features Regression (2019)|Mei & Montanari 2019]]). Implicit regularization of SGD + over-parameterization does work the explicit penalties used to
- **Over-parameterization as search:** dense random networks contain sparse **winning tickets** — (structure, init) pairs trainable to full accuracy at 10–20% of the size ([[The Lottery Ticket Hypothesis (2018)|Frankle 2018]]); big networks win partly by drawing many lottery tickets. Same logic observed in transformer heads ([[Are Sixteen Heads Really Better than One (2019)|Michel 2019]]) and MoE capacity
- LLM-era shift: ~1-epoch training on internet-scale data made classical overfitting rare — regularization moved from dropout to weight decay + data scale; loss follows scaling laws in $N, D$ ([[Scaling Laws for Neural Language Models (2020)|Kaplan 2020]])

## The architecture zoo (this vault's map)

MLP/feedforward (this note) → CNNs (weight sharing over space; enabled by [[Deep Residual Learning for Image Recognition (2015)|residuals]] at depth) → RNNs (sharing over time; superseded) → **[[Transformer]]** (attention + FFN — where every other note in this cluster lives) → post-transformer candidates ([[Linear Attention]]/[[Mamba (2023)|SSMs]]). The [[Feedforward Network]] inside a transformer *is* this note's object, two layers wide.

## Related

- Components: [[Activation Function]], [[Weight Initialization]], [[Residual Connection]], [[Normalization]]
- Training: [[Backpropagation]], [[Gradient Descent]], [[Adam Optimizer]]; theory: [[Neural Tangent Kernel]]
- The dominant instance: [[Transformer]]

## Sources

- [[Multilayer Feedforward Networks are Universal Approximators (1989)]] — expressivity foundation
- [[Understanding Deep Neural Networks with Rectified Linear Units (2016)]] — depth separation
- [[Dropout (2014)]] — classical regularization
- [[The Lottery Ticket Hypothesis (2018)]] — over-parameterization as search
- [[The Generalization Error of Random Features Regression (2019)]] — double descent
- [[Learning Representations by Back-propagating Errors (1986)]] — the training origin

---
Root of the foundations cluster.
