---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [kernel attention, linear transformer]
---

# Linear Attention

Kernel reformulation of the [[Attention Mechanism]] that removes the softmax to achieve $O(T)$ time and **constant-size decode state** — trading exact recall for efficiency, and eventually rebirthing as the SSM/Mamba line.

## The trick

Replace $\text{softmax}(qk^\top)$ similarity with a kernel $\text{sim}(q,k) = \phi(q)^\top \phi(k)$ (e.g. $\phi(x) = \text{elu}(x) + 1$). Associativity then re-brackets the computation ([[Transformers are RNNs - Linear Attention (2020)|Katharopoulos 2020]]):

$$\underbrace{(\phi(Q)\phi(K)^\top)}_{T \times T}V = \phi(Q)\underbrace{(\phi(K)^\top V)}_{d \times d}$$

The $T \times T$ matrix never exists. The causal form is a **recurrence** over a matrix-valued state:

$$S_t = S_{t-1} + \phi(k_t)v_t^\top, \qquad o_t = \frac{\phi(q_t)^\top S_t}{\phi(q_t)^\top z_t}$$

— a transformer that *is* an RNN at inference: $O(1)$ memory per token, **no growing [[KV Cache]]**. Results: up to **4000× faster** autoregressive generation on long sequences; ≈ vanilla-transformer quality *at the small scales tested*. (Performer's random features later made the kernel an unbiased softmax approximation — same family.)

## Why it lost round one — the recall bottleneck

- **The law** ([[BASED - Recall-Throughput Tradeoff (2024)|Arora 2024]]): there is a fundamental **state-size ↔ recall tradeoff**. Attention's growing cache = perfect recall, heavy memory; a fixed $d \times d$ state = a *lossy summary* of unbounded context — fixed-state models (linear attention, H3, RWKV, Mamba) provably and measurably **struggle at recall-intensive tasks** (grounding output in specific earlier tokens)
- Controlled pretraining comparisons: efficient attentions, linear included, failed to beat even a plain local window ([[Simple Local Attentions Remain Competitive (2021)|Xiong 2021]]); [[Flash Attention]] then removed the speed argument for approximating at all
- Additional practical failure: naive linear-attention implementations weren't IO-aware and ran *slower* than optimized softmax attention despite better asymptotics ([[Gated Linear Attention (2023)|Yang 2023]])

## The rebirth — selectivity and hybrids

- **Gating/selectivity:** the fix for the dumb additive state is **input-dependent forgetting**. GLA adds data-dependent gates to the state update + IO-aware FlashLinearAttention (faster than FlashAttention-2 even at 1K length); competitive with LLaMA-architecture transformers at moderate scale, and trained at 2K it generalizes past **20K** ([[Gated Linear Attention (2023)|Yang 2023]]). [[Mamba (2023)|Mamba]] is the same idea in SSM form — input-dependent $(\Delta, B, C)$ ("selective" state space) + a hardware-aware parallel scan: **5× generation throughput**, Mamba-3B matches transformers 2× its size at the scales tested. Gated linear attention ≡ selective SSM ≡ decaying RetNet — one family: *recurrent matrix state with learned forgetting*
- **Hybrids — the practical resolution:** compose a compressed global state with exact local attention. **BASED** (linear + tiny [[Sliding Window Attention|SWA]]): traverses the recall-memory Pareto frontier, +6.22 pts on recall tasks over Mamba, 24× FA2's generation throughput ([[BASED - Recall-Throughput Tradeoff (2024)]]). **Samba** (Mamba layers ⊕ SWA layers, 3.8B/3.2T): perfect 256K passkey recall from 4K training, 3.73× throughput vs GQA transformers ([[Samba (2024)|Ren 2024]]). Griffin/Jamba-style production hybrids follow the same recipe
- **Where it stands:** pure linear attention remains absent from frontier LLMs; *as an ingredient*, its recurrent state is everywhere the long-context/throughput frontier is being pushed

## Related

- Variant of the [[Attention Mechanism]] — the "change what's computed" escape from the [[On The Computational Complexity of Self-Attention (2022)|SETH]] quadratic wall
- Eliminates the [[KV Cache]] at the price [[BASED - Recall-Throughput Tradeoff (2024)]] quantifies
- Future notes: [[State Space Models]], [[Mamba]]
- IO-aware kernel doctrine inherited from [[Flash Attention]]

## Sources

- [[Transformers are RNNs - Linear Attention (2020)]] — the kernel trick + RNN equivalence
- [[Gated Linear Attention (2023)]] — gating + IO-aware kernels
- [[Mamba (2023)]] — selectivity at scale
- [[BASED - Recall-Throughput Tradeoff (2024)]] — the governing law
- [[Samba (2024)]] — the hybrid resolution
- [[Simple Local Attentions Remain Competitive (2021)]], [[Efficient Transformers - A Survey (2020)]] — round-one failure record

---
Part of the [[Transformer]] cluster
