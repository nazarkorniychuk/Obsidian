---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [MHA, attention heads, attention head]
---

# Multi-Head Attention

The original [[Attention Mechanism]] configuration ([[Attention Is All You Need (2017)|Vaswani 2017]]): run $h$ attention operations in parallel on **learned low-rank projections** of the input, concatenate, project back:

$$\text{head}_i = \text{Attention}(xW_Q^i, xW_K^i, xW_V^i), \qquad \text{MHA}(x) = \text{Concat}(\text{head}_1 \dots \text{head}_h)\,W_O$$

with $W^i \in \mathbb{R}^{d \times d_{head}}$, $d_{head} = d/h$ — so total FLOPs and parameters equal a *single* full-width head. What multiplies is not cost but **independent attention patterns**: each head has its own similarity geometry and can route different information. GPT-3: $h{=}96$, $d_{head}{=}128$; original Transformer: $h{=}8$, $d_{head}{=}64$.

**Why one head isn't enough:** softmax attention produces one weighted average per position — a single "vote" about where to look. Tasks need simultaneous relations (previous token *and* matching bracket *and* subject noun); one distribution can't serve all masters, $h$ separate ones can.

## Redundancy — most heads aren't needed (after training)

- **A large percentage of trained heads can be pruned at test time with no significant loss; some layers reduce to a single head** ([[Are Sixteen Heads Really Better than One (2019)|Michel 2019]]). Encoder-decoder cross-attention heads are most essential; self-attention heads most expendable
- Sharper: **38 of 48 encoder heads pruned at −0.15 BLEU** using stochastic-gate $L_0$ pruning ([[Analyzing Multi-Head Self-Attention (2019)|Voita 2019]])
- **The resolution of the paradox:** the surviving evidence points at **training dynamics** — many heads function as optimization scaffolding (a lottery-ticket-like over-provisioning) rather than as needed inference machinery ([[Are Sixteen Heads Really Better than One (2019)|Michel 2019]]). Train wide, prune later
- Practical echo: head redundancy is *why* [[Grouped-Query Attention]] (share KV across heads) and head-pruning accelerators work at all

## Specialization — the heads that matter have jobs

- **Interpretable roles** ([[Analyzing Multi-Head Self-Attention (2019)|Voita 2019]]): the important heads are consistently **positional** (fixed relative offset), **syntactic** (dependency relations: subject→verb, adjective→noun), or **rare-token** trackers — and specialized heads are the *last* to be pruned: importance ≡ interpretability
- **Induction heads** ([[In-context Learning and Induction Heads (2022)|Olsson 2022]]): a two-head circuit implementing match-and-copy (`[A][B]…[A] → [B]`); they **emerge abruptly at the training phase-change where in-context learning appears**, with multi-line evidence they are the mechanistic source of most ICL — the sharpest known answer to "what is a head *for*," and the mechanism behind [[Language Models are Few-Shot Learners - GPT-3 (2020)|GPT-3]]-era emergent ICL
- Caveat for all head-reading: attention weights ≠ importance ([[Quantifying Attention Flow (2020)|Abnar 2020]]) — role claims need ablation/causal evidence, which the above provide

## The rank tradeoff — the hidden price of $d_{head} = d/h$

Splitting $d$ into $h$ rank-$d/h$ heads is **not** a free reparameterization: there exist functions a single full-rank head represents at any context length that low-rank attention cannot approximate without **exponentially many heads** in $d$ ([[On the Benefits of Rank in Attention Layers (2024)|Amsel 2024]]); depth compensates only for short contexts. Practical relevance: head width is a real design axis (modern models hold $d_{head} \approx 128$ while scaling $h$ with $d$), and restoring per-head expressiveness is part of why [[Multi-Head Latent Attention|MLA]] can beat KV-sharing at equal cache.

## Hardware & variants

- MHA is embarrassingly parallel across heads — the batch dimension GPU kernels want; [[Flash Attention]] parallelizes over (batch × heads × sequence)
- At decode, per-head K/V is exactly what bloats the [[KV Cache]] ($n_{kv} = h$) — the MHA→MQA→[[Grouped-Query Attention|GQA]]→[[Multi-Head Latent Attention|MLA]] lineage exists to cut it; head pruning research fed accelerator designs (cascade token+head pruning ASICs)

## Related

- Baseline configuration of the [[Attention Mechanism]]; cache-reduction successors in [[Grouped-Query Attention]] and [[Multi-Head Latent Attention]]
- Induction heads ground in-context learning ([[Language Models are Few-Shot Learners - GPT-3 (2020)]])
- Head-level reading complements the FFN's [[Transformer Feed-Forward Layers Are Key-Value Memories (2021)|key-value memory]] reading — the two interpretability entry points of the [[Transformer]]

## Sources

- [[Attention Is All You Need (2017)]] — the formulation
- [[Are Sixteen Heads Really Better than One (2019)]] — redundancy; training-dynamics hypothesis
- [[Analyzing Multi-Head Self-Attention (2019)]] — specialization + pruning
- [[In-context Learning and Induction Heads (2022)]] — induction heads / ICL
- [[On the Benefits of Rank in Attention Layers (2024)]] — the rank-vs-heads theorem
- [[Quantifying Attention Flow (2020)]] — the interpretability caveat

---
Part of the [[Transformer]] cluster
