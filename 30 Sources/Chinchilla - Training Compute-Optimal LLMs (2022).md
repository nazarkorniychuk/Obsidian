---
type: source
source-type: paper
authors: [Jordan Hoffmann, et al. (DeepMind)]
year: 2022
url: https://consensus.app/papers/details/8878a8d11742580fa23cc42a5e9f2a55/?utm_source=claude_desktop
doi: 10.52202/068431-2176
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Chinchilla paper, Hoffmann 2022]
---

# Chinchilla - Training Compute-Optimal LLMs (2022)

*Full title: "Training Compute-Optimal Large Language Models"* (~3,600 citations).

## Summary

Redid the [[Scaling Laws for Neural Language Models (2020)|Kaplan]] compute-allocation analysis with 400+ models (70M–16B, 5–500B tokens) and found the field's LLMs were **significantly undertrained**: compute-optimally, **model size and training tokens should scale equally** — double the model, double the data (≈ **20 tokens per parameter**).

## Key results

- **Chinchilla-70B**, trained on 4× the data of Gopher-280B at the *same compute*, **uniformly beats Gopher, GPT-3 (175B), Jurassic-1 (178B), and MT-NLG (530B)** — e.g. 67.5% MMLU (+7 over Gopher)
- Bonus: a compute-optimal *smaller* model is also cheaper to fine-tune and serve

## Impact

Reset the industry's training recipes (LLaMA-1's "small model, lots of tokens" philosophy is Chinchilla-reasoning pushed further — modern models train far *past* 20 tok/param because inference cost, not training compute, dominates the bill). The canonical example that scaling *laws* are empirical fits, revisable by better experiments.

## Concepts extracted

- [[Transformer]] — compute-optimal scaling
