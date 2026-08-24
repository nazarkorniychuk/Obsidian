---
type: concept
topics:
  - deep-learning
status: seedling
created: 2026-08-22
aliases:
  - MoE
---

# Mixture of Experts

## Related

- Variant of [[Feedforward Network]] — replaces the dense FFN with routed experts
- Component of [[Transformer]]

## Sources (collected, for the future write-up)

- [[Outrageously Large Neural Networks (2017)]] — origin: sparse gating, top-k, load-balancing losses
- [[Switch Transformers (2021)]] — top-1 routing; 7× speedup; 1.6T params
- [[Mixtral of Experts (2024)]] — open-weights: 47B/13B active, beats LLaMA-2-70B
- [[DeepSeek-V3 Technical Report (2024)]] — 671B/37B; fine-grained + shared experts; loss-free balancing
- [[Sparsity Moves Computation (2026)]] — random ≈ learned routing (toy scale)
- Future children: [[Expert Routing]], load balancing
