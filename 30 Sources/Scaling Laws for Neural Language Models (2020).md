---
type: source
source-type: paper
authors: [Jared Kaplan, Sam McCandlish, Tom Henighan, Tom B. Brown, Benjamin Chess, Rewon Child, Scott Gray, Alec Radford, Jeffrey Wu, Dario Amodei]
year: 2020
url: https://consensus.app/papers/details/03a6af89f8865839b8e1fb6cc6ea9a3f/?utm_source=claude_desktop
doi: 10.48550/arxiv.2001.08361
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Kaplan 2020, scaling laws paper]
---

# Scaling Laws for Neural Language Models (2020)

(~8,800 citations. OpenAI.)

## Summary

Cross-entropy loss of transformer LMs follows smooth **power laws** in model size $N$, dataset size $D$, and compute $C$ — spanning 7+ orders of magnitude:

$$L(N) \propto N^{-\alpha_N}, \quad L(D) \propto D^{-\alpha_D}, \quad L(C) \propto C^{-\alpha_C}$$

with $\alpha_N \approx 0.076$, $\alpha_D \approx 0.095$.

## Key results

- **Architecture details (width vs depth, shape) barely matter** within wide ranges — scale is the dominant variable; the finding that redirected the field from architecture search to scaling
- Larger models are **more sample-efficient**; compute-optimal training = very large models stopped well before convergence
- Simple equations govern overfitting as a function of $N/D$

## Impact

The intellectual justification for GPT-3 ([[Language Models are Few-Shot Learners - GPT-3 (2020)]]) and the scaling era. Its compute-allocation prescription (grow $N$ much faster than $D$) was later **corrected** by [[Chinchilla - Training Compute-Optimal LLMs (2022)]].

## Concepts extracted

- [[Transformer]] — scaling laws section; comparison point for [[Mixture of Experts]] scaling laws
