---
type: source
source-type: paper
authors: [Angelos Katharopoulos, Apoorv Vyas, Nikolaos Pappas, François Fleuret]
year: 2020
url: https://consensus.app/papers/details/af96257518d65dc8b75adff0d22a18ff/?utm_source=claude_desktop
doi: 10.48550/arxiv.2006.16236
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [linear attention paper, Katharopoulos 2020]
---

# Transformers are RNNs - Linear Attention (2020)

*Full title: "Transformers are RNNs: Fast Autoregressive Transformers with Linear Attention"* (~3,100 citations).

## Summary

Replace $\text{softmax}(QK^\top)V$ with a **kernel factorization**: $\text{sim}(q,k) = \phi(q)^\top \phi(k)$ (they use $\phi(x) = \text{elu}(x)+1$). Then associativity re-brackets the computation:

$$\underbrace{(\phi(Q)\,\phi(K)^\top)}_{T \times T}\,V \;=\; \phi(Q)\,\underbrace{(\phi(K)^\top V)}_{d \times d}$$

— the $T\times T$ matrix never exists; complexity drops $O(T^2) \to O(T)$. Causal version becomes a **recurrence** over a running $d\times d$ state $S_t = S_{t-1} + \phi(k_t)v_t^\top$: a transformer that *is* an RNN at inference — $O(1)$ memory per new token, no KV cache growth.

## Key results

- **Up to 4000× faster autoregressive generation** on very long sequences
- "Similar performance to vanilla transformers" on the tasks tested (image generation, speech) — *at small scale*

## The failure that matters

At LLM scale the fixed $d \times d$ state can't losslessly store a growing context — quality gaps on recall-heavy tasks kept pure linear attention out of frontier LLMs (see [[Simple Local Attentions Remain Competitive (2021)]], [[Efficient Transformers - A Survey (2020)]]). Its true legacy: the recurrent-state formulation is the intellectual ancestor of the SSM line (Mamba-style models) and of hybrid architectures.

## Concepts extracted

- [[Attention Mechanism]] — linear/kernel family
