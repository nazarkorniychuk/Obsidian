---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [Xavier initialization, He initialization, muP]
---

# Weight Initialization

Where [[Gradient Descent]] starts determines whether it can proceed at all: initialization schemes exist to make signal and gradient magnitudes **depth-stable at step 0**. The founding empirical claim: historical "deep nets can't be trained" failures were **initialization failures** ([[On the Importance of Initialization and Momentum (2013)|Sutskever 2013]]).

## The variance-propagation principle

Through a dense layer $z = Wx$ ($n_{in}$ inputs, i.i.d. weights), $\text{Var}(z_i) = n_{in}\text{Var}(W)\text{Var}(x_i)$. Stacked over $L$ layers, variance multiplies by $(n\,\text{Var}(W))^L$ — anything but $\approx 1$ per layer explodes or vanishes geometrically, in both the forward pass and the [[Backpropagation|backward]] pass (layer Jacobian singular values far from 1). Init schemes are solutions of "set $\text{Var}(W)$ so the product is 1":

| Scheme | $\text{Var}(W)$ | Derived for | Source |
| --- | --- | --- | --- |
| **Xavier/Glorot** | $\dfrac{2}{n_{in}+n_{out}}$ | tanh-era symmetric activations; balances forward & backward | [[Understanding the Difficulty of Training Deep Feedforward Networks (2010)\|Glorot & Bengio 2010]] |
| **He/Kaiming** | $\dfrac{2}{n_{in}}$ | ReLU (zeroing half the pre-activations halves variance → factor 2) | [[Delving Deep into Rectifiers (2015)\|He 2015]] |

Glorot's paper also diagnosed *why* pre-2010 training failed (sigmoid's non-zero mean saturating top layers) and established variance-tracking as the method; He init enabled training very deep rectifier nets from scratch and remains the PyTorch default. Zero init is forbidden for symmetric layers (all units compute identical functions forever — symmetry never breaks under identical gradients).

## Transformer-specific rules

- **Residual accumulation:** $L$ blocks *add* into the stream ([[Residual Connection]]) → output variance grows with depth even with perfect per-layer init. Standard fix: scale residual-branch output projections by $1/\sqrt{2L}$ (GPT-2 convention); principled versions: **DeepNorm**'s derived constants train **1,000 layers** ([[DeepNet - Scaling Transformers to 1000 Layers (2022)|Wang 2022]]), LayerNorm-scaling counters pre-LN's depth curse ([[The Curse of Depth in LLMs (2025)|Sun 2025]])
- Embeddings and LN gains get their own conventions ($\mathcal{N}(0, 0.02)$, ones) — small details, large stability effects

## μP — initialization as a *scaling law*

In standard parameterization, optimal hyperparameters **drift as width grows** — small-model tuning misleads. **μP (Maximal Update Parametrization)** rescales init variances *and* per-layer learning rates with width so optimal HPs become **width-stable** → **μTransfer**: tune at toy scale, transfer zero-shot. Results: HPs from a **13M** proxy beat published BERT-large (350M); from **40M**, beat published GPT-3-6.7B numbers at **7% of pre-training cost** in tuning ([[Tensor Programs V - muTransfer (2022)|Yang 2022]]). Theoretically, μP is the unique width-scaling that preserves **feature learning** at infinite width — the escape from the frozen-features [[Neural Tangent Kernel|NTK]] regime. Standard practice at frontier labs; depth-μP and per-optimizer spectral extensions ongoing.

## The deeper point — init is destiny

- Momentum can't rescue bad init; good init + momentum ≈ second-order methods ([[On the Importance of Initialization and Momentum (2013)]])
- **Lottery tickets:** sparse subnetworks train to full accuracy *only with their original initialization* — the winning ticket is the (structure, init) pair; over-parameterization works partly as an initialization lottery ([[The Lottery Ticket Hypothesis (2018)|Frankle 2018]])
- Whether a network lands in the lazy NTK regime or the feature-learning regime is set by the parameterization — init scale is a *regime selector*, not a detail

## Related

- Enables [[Gradient Descent]]/[[AdamW]]; stabilizes [[Backpropagation]]
- Interlocks with [[Activation Function]] (He's factor 2 is ReLU's), [[Residual Connection]] + [[Normalization]] (depth scaling), [[Neural Tangent Kernel]] (regime)

## Sources

- [[Understanding the Difficulty of Training Deep Feedforward Networks (2010)]] — Xavier + the diagnosis
- [[Delving Deep into Rectifiers (2015)]] — He init
- [[On the Importance of Initialization and Momentum (2013)]] — init×momentum
- [[Tensor Programs V - muTransfer (2022)]] — μP
- [[The Lottery Ticket Hypothesis (2018)]] — init as destiny
- [[DeepNet - Scaling Transformers to 1000 Layers (2022)]], [[The Curse of Depth in LLMs (2025)]] — transformer depth scaling

---
Part of the foundations cluster.
