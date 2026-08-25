---
type: concept
topics: [deep-learning]
status: evergreen
created: 2026-08-25
aliases: [SwiGLU, GEGLU, ReGLU, Gated Linear Unit]
---

# GLU Variants

The gated [[Feedforward Network]] family: two input projections, one element-wise gating the other. The variant = the gate's [[Activation Function]]:

$$\text{GLU}(x) = (W_1 x) \otimes \sigma(W_2 x) \;\;\text{(sigmoid gate, Dauphin 2016)}$$
$$\text{ReGLU}: \text{ReLU gate} \qquad \text{GEGLU}: \text{GELU gate} \qquad \textbf{SwiGLU}: \text{Swish gate}$$

followed by down-projection $W_3$. Full notation, dimensions, and the per-unit "value × gate" reading: [[Feedforward Network]] → Gated FFN section.

## Results

- **At fixed parameters** ($d_{ff}$ shrunk to 8/3 of dense to pay for the third matrix), GEGLU and SwiGLU **beat both ReLU and GELU FFNs** on T5 pre-training perplexity and GLUE/SuperGLUE fine-tuning — offered with "no explanation… other than divine benevolence" ([[GLU Variants Improve Transformer (2020)|Shazeer 2020]])
- **The explanation, six years later:** gating **shrinks the [[Neural Tangent Kernel|NTK]] condition number** (compacter eigenvalue spectrum) → faster optimization; ≈ no change to the generalization gap — a trainability win, not a better function class ([[The Devil is in the Condition Numbers (2026)|Lyu 2026]])
- **Costs:** two input matrices = **2× memory reads** at bandwidth-bound decode; recoverable — masked single-matrix SwiMGLU matches SwiGLU accuracy at 47% less traffic ([[Masked Gated Linear Unit (2025)|Tajima 2025]]). Side effect: gating rotates features off the neuron basis, degrading neuron-level interpretability ([[Sparsity Moves Computation (2026)|Smithline 2026]])

## Adoption

**SwiGLU:** PaLM, LLaMA 1–3, Mistral, Qwen, DeepSeek (and their MoE experts — [[Mixture of Experts]]). **GEGLU:** Gemma. The de-facto FFN of every current LLM; LLaMA-7B's $d_{ff} = 11008 \approx \tfrac{8}{3} \cdot 4096$ is Shazeer's param-matching made flesh.

## Related

- Layer-level redesign of the [[Feedforward Network]]; gate function from the [[Activation Function]] family
- Mechanism via [[Neural Tangent Kernel]] conditioning
- Expert FFNs in [[Mixture of Experts]] are usually SwiGLU

## Sources

- [[GLU Variants Improve Transformer (2020)]] — the family + results
- [[The Devil is in the Condition Numbers (2026)]] — the mechanism
- [[Masked Gated Linear Unit (2025)]] — the bandwidth cost + fix
- [[Sparsity Moves Computation (2026)]] — interpretability side effect

---
Part of the [[Transformer]] cluster
