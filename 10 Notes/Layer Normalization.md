---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [LayerNorm, LN, RMSNorm]
---

# Layer Normalization

Per-token activation normalization — the [[Transformer]]'s training stabilizer, whose *placement* turned out to matter more than its formula.

$$\text{LN}(x) = \gamma \odot \frac{x - \mu(x)}{\sigma(x)} + \beta \qquad\qquad \text{RMSNorm}(x) = \gamma \odot \frac{x}{\sqrt{\tfrac{1}{d}\sum_i x_i^2}}$$

- Statistics are over the **feature dimension of one token** (unlike batch norm — no batch dependence, works at batch 1 and with autoregression)
- **RMSNorm** drops the mean-centering (and $\beta$) — cheaper, and effectively equivalent in the pre-LN transformer since the residual stream's mean component is redundant there; **the LLM default** (LLaMA-family, Mistral, Qwen)

## Placement — the actual story

- **Post-LN** (original Transformer: $\text{LN}(x + f(x))$): at initialization the gradients near the output are **large** → training diverges at real learning rates → the mysterious **warmup stage** exists solely to survive this ([[On Layer Normalization in the Transformer Architecture (2020)|Xiong 2020]], mean-field proof)
- **Pre-LN** ($x + f(\text{LN}(x))$): LN moves *off the residual path* → gradients well-behaved at init → **warmup removable**, deep training stable. The GPT-2-onward default
- **Pre-LN's hidden cost — the curse of depth:** the residual stream's variance grows with depth, so each deep block's normalized contribution shrinks — **deep-layer derivatives approach identity and nearly half the layers of Llama/Mistral/DeepSeek/Qwen-class models under-contribute**; fixable by scaling LN output by $1/\sqrt{\ell}$ (LayerNorm Scaling), which beats prior schemes on 130M–7B pre-training ([[The Curse of Depth in LLMs (2025)|Sun 2025]])
- **Fixing post-LN instead:** **DeepNorm** ($\text{LN}(\alpha x + f(x))$ with derived constants) bounds updates → **1,000-layer transformers** train cleanly; a 200-layer/3.2B model beat a 48-layer/12B SOTA by 5 BLEU on massively multilingual MT ([[DeepNet - Scaling Transformers to 1000 Layers (2022)|Wang 2022]])
- Newer placements (Peri-LN, hybrid QKV-norm + post-FFN-norm) chase the same tradeoff: pre-LN's gradient flow *and* post-LN's per-layer effectiveness

## What LN actually does (beyond stability)

- **Expressivity role:** geometrically LN = projection onto the hyperplane ⊥ $[1,\dots,1]$ + rescale to norm $\sqrt d$ — the projection lets attention form queries that attend uniformly, the rescaling keeps every key *selectable* (no "un-attendable" tokens)
- **Rank collapse interaction:** with the right value matrices, attention+LN can sustain equilibria of *any* rank — LN is part of why real transformers escape pure attention's rank-1 collapse ([[Attention is Not All You Need - Rank Collapse (2021)|Dong 2021]] and follow-ups)
- **Maybe it's just a squash:** trained LNs implement tanh-like S-curves; replacing all normalization with $\tanh(\alpha x)$ (**DyT**, a learned scalar, no statistics) **matches or beats normalized transformers** across vision and language ([[Transformers without Normalization (2025)|Zhu 2025]]) — suggesting the per-token statistics were never the essential part

## Hardware note

LN is a reduction (mean/variance across $d$) — cheap in FLOPs but a kernel-fusion nuisance (sync point between GEMMs); RMSNorm halves the reductions, DyT removes them entirely — part of these variants' appeal.

## Related

- One of the two glue components of the [[Transformer]] block (with [[Residual Connection]] — placement relative to the skip *is* the pre/post-LN question)
- Softmax-numerics cousins: router z-loss ([[ST-MoE (2022)]]), logit capping ([[Unembedding]])

## Sources

- [[On Layer Normalization in the Transformer Architecture (2020)]] — pre vs post, warmup
- [[The Curse of Depth in LLMs (2025)]] — pre-LN's depth pathology
- [[DeepNet - Scaling Transformers to 1000 Layers (2022)]] — post-LN at 1000 layers
- [[Transformers without Normalization (2025)]] — DyT
- [[Attention is Not All You Need - Rank Collapse (2021)]] — LN in collapse dynamics

---
Part of the [[Transformer]] cluster
