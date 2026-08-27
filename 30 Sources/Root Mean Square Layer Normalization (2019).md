---
type: source
source-type: paper
authors: [Biao Zhang, Rico Sennrich]
year: 2019
url: https://consensus.app/papers/details/7bb4661bfc065fc0b7fad9be5ccf2580/?utm_source=claude_desktop
doi: 10.5167/uzh-177483
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [RMSNorm paper, Zhang & Sennrich 2019]
---

# Root Mean Square Layer Normalization (2019)

(NeurIPS 2019, ~1,900 citations.)

## Summary

Hypothesis: LayerNorm's **re-centering (mean subtraction) is dispensable** — only the re-scaling matters. **RMSNorm** keeps just the scale part:

$$\text{RMSNorm}(x) = \gamma \odot \frac{x}{\sqrt{\tfrac{1}{d}\sum_i x_i^2 + \epsilon}}$$

No mean, no bias $\beta$ — one reduction instead of two, retaining re-scaling invariance and implicit learning-rate adaptation.

## Key results

- **Comparable quality to LayerNorm across tasks and architectures** while cutting running time **7–64%** depending on the model
- pRMSNorm variant estimates the RMS from a subset of dimensions

## Impact

**The LLM default**: LLaMA 1–3, Mistral, Qwen, DeepSeek, Gemma all use RMSNorm (in pre-norm placement). Later shown formally that in pre-LN transformers the residual stream's mean component is redundant — Pre-LN and Pre-RMSNorm are convertible/equivalent (Gu 2023), retroactively justifying the "re-centering is dispensable" hypothesis for exactly the architecture that adopted it.

## Concepts extracted

- [[Normalization]] — the LLM-standard variant; placement details in [[Normalization]]
