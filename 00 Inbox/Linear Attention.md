---
type: concept
topics: [deep-learning]
status: seedling
created: 2026-08-24
aliases: [kernel attention, linear transformer]
---

# Linear Attention

## Related

- Variant of [[Attention Mechanism]] — kernel factorization $\phi(Q)(\phi(K)^\top V)$, $O(T)$ time, recurrent $d{\times}d$ state at decode
- To cover in depth: kernel choices ($\text{elu}{+}1$, random features/Performer), the RNN equivalence, the fixed-state recall bottleneck (why it lost at LLM scale), the rebirth: SSMs/Mamba, gated linear attention, hybrid dense+linear architectures (Jamba-style)
- Sources ready: [[Transformers are RNNs - Linear Attention (2020)]], [[Efficient Transformers - A Survey (2020)]], [[Simple Local Attentions Remain Competitive (2021)]]
- Future siblings: [[State Space Models]], [[Mamba]]
