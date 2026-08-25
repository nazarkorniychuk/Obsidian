---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-22
aliases: [Attention, Self-Attention, Scaled Dot-Product Attention]
---

# Attention Mechanism

The token-mixing component of the [[Transformer]] — the only place information moves **between positions** (the [[Feedforward Network]] transforms each position independently):

$$\text{Attention}(Q,K,V) = \text{softmax}\!\left(\frac{QK^{\top}}{\sqrt{d_k}}\right)V$$

$$\text{MHA}(x) = \text{Concat}(\text{head}_1, \dots, \text{head}_h)\,W_O, \qquad \text{head}_i = \text{Attention}(xW_Q^i,\, xW_K^i,\, xW_V^i)$$

Notation:
- $Q, K, V \in \mathbb{R}^{T \times d_k}$ — queries, keys, values: three learned projections of the same token matrix; entry $(i,j)$ of $QK^\top$ scores how much position $i$ should read from position $j$
- $\sqrt{d_k}$ **scaling:** for random $q,k$ with unit-variance entries, $\text{Var}(q \cdot k) = d_k$ — unscaled logits grow with head size, saturating the softmax into near-one-hot → vanishing gradients ([[Attention Is All You Need (2017)|Vaswani 2017]]'s stated reason)
- **Heads:** $h$ heads of width $d_{head} = d/h$ (e.g. GPT-3: $h{=}96$, $d_{head}{=}128$) — same total FLOPs as one wide head, but $h$ independent attention patterns
- **Complexity:** time $O(T^2 d)$; naive memory $O(T^2)$ for the score matrix (fixed by [[FlashAttention (2022)]]); provably no exact sub-quadratic algorithm unless SETH fails ([[On The Computational Complexity of Self-Attention (2022)|Keles 2022]])
- **[[KV Cache]] per token** $= 2 \cdot L \cdot n_{kv} \cdot d_{head} \cdot \text{bytes}$ ($L$ layers, $n_{kv}$ KV heads; the 2 = K and V) — the quantity the entire MQA/GQA/MLA lineage exists to shrink

![[attn-kv-cache.png]]

## ⚡ Adoption table

| Variant | Introduced by | Flagship adopters | Reason |
| --- | --- | --- | --- |
| MHA | [[Attention Is All You Need (2017)\|Vaswani 2017]] | **BERT, GPT-1/2, LLaMA-1, ViT** | the baseline; $h$ independent patterns at one head's FLOPs |
| Dense + banded-sparse alternation | Sparse-Transformer pattern; shipped in [[Language Models are Few-Shot Learners - GPT-3 (2020)\|GPT-3]] | **GPT-3 (96L, 96h)** | first production sparse attention |
| MQA ($n_{kv}{=}1$) | [[Fast Transformer Decoding - One Write-Head is All You Need (2019)\|Shazeer 2019]] | **PaLM, Falcon** | $h\times$ smaller KV cache; minor quality cost |
| GQA ($1 < n_{kv} < h$) | [[GQA (2023)\|Ainslie 2023]] | **LLaMA-2-70B/3, Mistral, Qwen2, Gemma-2** | MHA quality at ≈MQA speed; 5%-compute uptraining from MHA checkpoints |
| MLA (latent compression) | [[DeepSeek-V2 (2024)\|DeepSeek-AI 2024]] | **DeepSeek-V2/V3** | 93.3% KV reduction; 5.76× generation throughput |
| CLA (cross-layer KV sharing) | [[Reducing Transformer KV Cache with Cross-Layer Attention (2024)\|Brandon 2024]] | research | 2× further beyond MQA at ≈accuracy |
| Sliding window ($O(Tw)$) | [[Longformer (2020)\|Beltagy 2020]] | **Mistral-7B (w=4096), Gemma-2/3 local-global hybrids** | linear cost; receptive field grows with depth |
| Linear/kernel attention ($O(T)$) | [[Transformers are RNNs - Linear Attention (2020)\|Katharopoulos 2020]] | SSM/Mamba lineage, hybrids | constant-memory recurrent decode; quality gap kept it from frontier LLMs |
| FlashAttention (exact, IO-aware) | [[FlashAttention (2022)\|Dao 2022]] / [[FlashAttention-2 (2023)\|Dao 2023]] | **universal** (PyTorch SDPA, every LLM stack) | implementation, not a variant: same math, 2–4× faster, $O(T)$ memory |

## ⚡ Key numbers

- **KV cache** (32L, $d{=}4096$, $d_{head}{=}128$, fp16): MHA **512 KB/token** → GQA-8 **128 KB** → MLA **≈36 KB** → MQA **16 KB** → MQA+CLA2 **8 KB** (chart above). At 128k context, MHA needs 64 GB *per sequence* — why nobody ships MHA anymore
- **Attention vs FFN parameters:** QKVO $= 4d^2$ per block vs FFN $8d^2$ — attention is ⅓ of weights but, naively, $O(T^2)$ of the activation memory
- **FlashAttention-2:** 50–73% of A100 peak FLOPs; **225 TFLOPs/s per GPU (72% MFU)** end-to-end GPT training ([[FlashAttention-2 (2023)|Dao 2023]])
- **MQA at scale:** 32× longer context at fixed memory; PaLM 540B decodes at **29 ms/token** ([[Efficiently Scaling Transformer Inference (2022)|Pope 2022]])

---

## The KV-cache lineage — decode is a bandwidth problem

At each decode step the model re-reads every cached K and V. Cutting $n_{kv}$ cuts the bytes:

- **MQA** ([[Fast Transformer Decoding - One Write-Head is All You Need (2019)|Shazeer 2019]]): all $h$ query heads share one K and one V head → cache shrinks $h\times$. **Result:** much faster incremental decoding, *minor but real* quality drop (small BLEU loss on WMT En-De)
- **GQA** ([[GQA (2023)|Ainslie 2023]]): $g$ KV heads, each serving $h/g$ query heads — the MHA↔MQA dial. **Results:** uptraining an MHA checkpoint (mean-pool KV heads into groups, train 5% more compute) gives **quality ≈ MHA at speed ≈ MQA**. LLaMA-3 ships $g{=}8$
- **MLA** ([[DeepSeek-V2 (2024)|DeepSeek-AI 2024]]): don't share heads — **compress**. Down-project each token to a latent $c_t = W_{DKV}\,h_t \in \mathbb{R}^{d_c}$ with $d_c \ll h \cdot d_{head}$ (V2: $d_c{=}512$ vs $d{=}5120$); cache only $c_t$ (+ a small decoupled RoPE key, 64 dims); up-project to full K,V per head at use time (the up-projections fold into $W_Q$/$W_O$ — no extra decode FLOPs). **Results:** **93.3% KV cache reduction, 5.76× max generation throughput** vs DeepSeek-67B; quality *above* MHA baseline in their ablations
- **CLA** ([[Reducing Transformer KV Cache with Cross-Layer Attention (2024)|Brandon 2024]]): share the cached KV between adjacent **layers** — orthogonal third axis. **2× further reduction on top of MQA** at nearly unchanged accuracy (1B/3B from scratch); MLKV variants reach 6× below MQA
- **Conclusion:** head-sharing (GQA) won on simplicity; latent compression (MLA) wins on ratio; the axes compose

## Sparse & local attention

- **Sliding window** ([[Longformer (2020)|Beltagy 2020]]): each token attends to $w$ neighbors → $O(Tw)$; layer $\ell$'s receptive field is $\ell \cdot w$ (like stacked convolutions); + a few global tokens that see/are seen by everything. **Results:** SOTA text8/enwik8 char-LM; beat RoBERTa on every long-document task tested. Adopted: Mistral-7B ($w{=}4096$), Gemma-2/3 interleaved local:global layers
- **Fixed sparse patterns in production:** [[Language Models are Few-Shot Learners - GPT-3 (2020)|GPT-3]] alternated dense and locally-banded sparse layers — sparse attention shipped in the largest model of 2020, then quietly disappeared from GPT-4-era dense models once [[FlashAttention (2022)]] made dense cheap
- **Content-based sparsity** (clustering/LSH — Reformer, Routing Transformer): $O(T^{1.5})$–$O(T\log T)$; never displaced fixed windows (see failure story below)

## Linear attention — and the efficient-attention failure story

- **The trick** ([[Transformers are RNNs - Linear Attention (2020)|Katharopoulos 2020]]): replace softmax similarity with a kernel $\phi(q)^\top\phi(k)$; associativity re-brackets $(\phi(Q)\phi(K)^\top)V = \phi(Q)(\phi(K)^\top V)$ — the $T{\times}T$ matrix never exists, $O(T^2) \to O(T)$. Causal form is a recurrence over a $d{\times}d$ state $S_t = S_{t-1} + \phi(k_t)v_t^\top$: **constant memory per token, no growing KV cache**, up to 4000× faster long-sequence generation
- **Why it lost (as a full replacement):** the fixed-size state is a lossy summary of an unbounded context → recall degrades at scale. Controlled replication: **none of the efficient attentions beat a plain local window** under identical pretrain-finetune protocol, and the LRA benchmark that ranked them doesn't predict real-task performance ([[Simple Local Attentions Remain Competitive (2021)|Xiong 2021]]); disjoint local attention matched Longformer at half the pretraining compute
- **The ablation bracket:** replacing attention with a *parameter-free Fourier transform* keeps 92–97% of BERT-GLUE at 80% faster training ([[FNet (2021)|Lee-Thorp 2021]]) — most of attention's value on encoder tasks is "some fixed mixing"; the content-dependent last few % is what decoders can't do without
- **Post-mortem** (why the ~50-variant X-former zoo of [[Efficient Transformers - A Survey (2020)|Tay 2020]] went extinct at the frontier): (1) quality gaps at scale, (2) [[FlashAttention (2022)]] deleted the wall-clock argument for approximation, (3) exact sub-quadratic is impossible anyway ([[On The Computational Complexity of Self-Attention (2022)|Keles 2022]]). Survivors: sliding windows (hybridized) and linear attention's recurrence (reborn as the SSM/Mamba line)

## Theory — what attention provably can and cannot do

- **Upper bound (idealized):** transformers with arbitrary-precision activations are **Turing complete** ([[Attention is Turing-Complete (2021)|Pérez 2021]]) — but the construction needs unbounded precision/steps
- **Lower bounds (realistic):** fixed-depth self-attention **cannot recognize PARITY or Dyck languages** unless depth/heads grow with input length ([[Theoretical Limitations of Self-Attention (2020)|Hahn 2020]]) — no bounded transformer is a general sequence computer; matches observed LLM weakness at state-tracking
- **What attention is *for*:** on **sparse averaging** (retrieve-and-average $q$ pointed-to positions), transformer size scales $O(\log T)$ vs polynomial for RNNs/FFNs — the formal version of "attention does content-based long-range retrieval"; but **triple detection** (any 3-way relation among positions) costs attention $O(T)$ size — $QK^\top$ is pairwise-only machinery ([[Representational Strengths and Limitations of Transformers (2023)|Sanford 2023]])
- **Rank collapse:** pure attention stacks (no residuals, no FFN) converge to a **rank-1 output doubly exponentially in depth** — all tokens become identical; skip connections and the FFN provably prevent it ([[Attention is Not All You Need - Rank Collapse (2021)|Dong 2021]]). The [[Residual Connection]] and [[Feedforward Network]] are load-bearing counterweights, not conveniences
- **Attention weights ≠ explanations:** past the first layers, token identities are mixed into every position, so raw attention maps correlate poorly with input importance; use rollout ($\tilde A = \prod_\ell \tfrac{1}{2}(A^{(\ell)}{+}I)$) or flow instead ([[Quantifying Attention Flow (2020)|Abnar 2020]])

## Hardware

- **Training/prefill — the $T\times T$ problem:** naive attention writes the score matrix to HBM: $O(T^2)$ traffic dominates. [[FlashAttention (2022)|FlashAttention]] tiles Q/K/V through on-chip SRAM with **online softmax** (running max + sum, rescale per block) — exact result, $O(T)$ extra memory, provably fewer HBM accesses. Results: 15% over the BERT MLPerf record, 3× GPT-2, first >chance Path-X (16K). [[FlashAttention-2 (2023)|FA2]] fixes work partitioning: **50–73% of peak**, 225 TFLOPs/A100 end-to-end
- **Decode — the KV-cache problem:** per token, attention FLOPs are tiny but every cached K/V byte is re-read → bandwidth-bound; this is what the MQA/GQA/MLA lineage optimizes. Prefill and decode are different computations with different optimal parallelism ([[Efficiently Scaling Transformer Inference (2022)|Pope 2022]]: 76% MFU prefill vs 29 ms/token decode on PaLM 540B)
- **Strategic effect:** FlashAttention made *exact* attention nearly GEMM-fast, which killed approximate attention's raison d'être — hardware-aware implementation beat algorithmic approximation
- **MoE interaction:** the resident expert pool competes with the KV cache for HBM ([[The qs Inequality (2026)|Adhinarayanan 2026]]) — attention and FFN memory budgets are coupled at serving time

---

## Related

- Component of the [[Transformer]]; contrast with [[Feedforward Network]] — mixes across tokens vs transforms within a token; attention can even absorb the FFN's function via persistent memory vectors ([[Augmenting Self-attention with Persistent Memory (2019)|Sukhbaatar 2019]])
- [[KV Cache]] — the decode-time state this note's variant lineage shrinks
- [[Multi-Head Attention]], [[Grouped-Query Attention]], [[Multi-Head Latent Attention]], [[Sliding Window Attention]], [[Flash Attention]] — future per-variant deep dives
- [[Positional Encoding]] — attention is permutation-invariant without it; MLA's decoupled-RoPE key exists because RoPE doesn't commute with MLA's up-projections
- Softmax's gate role: see [[Activation Function]]

## Sources

- [[Attention Is All You Need (2017)]] — the mechanism, scaling factor, MHA
- [[Fast Transformer Decoding - One Write-Head is All You Need (2019)]] — MQA
- [[GQA (2023)]] — grouped heads + 5% uptraining
- [[DeepSeek-V2 (2024)]] — MLA latent compression
- [[Reducing Transformer KV Cache with Cross-Layer Attention (2024)]] — cross-layer sharing
- [[Efficiently Scaling Transformer Inference (2022)]] — MQA at scale; prefill/decode split
- [[Longformer (2020)]] — sliding window + global tokens
- [[Language Models are Few-Shot Learners - GPT-3 (2020)]] — production sparse attention; GPT-3 config
- [[Transformers are RNNs - Linear Attention (2020)]] — kernel/linear attention
- [[Efficient Transformers - A Survey (2020)]] — the X-former zoo
- [[Simple Local Attentions Remain Competitive (2021)]] — the controlled failure result
- [[FNet (2021)]] — Fourier ablation
- [[FlashAttention (2022)]] / [[FlashAttention-2 (2023)]] — IO-aware exact attention
- [[On The Computational Complexity of Self-Attention (2022)]] — SETH quadratic lower bound
- [[Attention is Turing-Complete (2021)]] — idealized upper bound
- [[Theoretical Limitations of Self-Attention (2020)]] — PARITY/Dyck impossibility
- [[Representational Strengths and Limitations of Transformers (2023)]] — sparse averaging vs triple detection
- [[Attention is Not All You Need - Rank Collapse (2021)]] — rank-1 collapse; residual/FFN necessity
- [[Quantifying Attention Flow (2020)]] — attention ≠ explanation
- [[Augmenting Self-attention with Persistent Memory (2019)]] — attention absorbing the FFN

---
Part of the [[Transformer]] cluster
