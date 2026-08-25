---
type: source
source-type: paper
authors: [Barret Zoph, Irwan Bello, William Fedus, et al. (Google)]
year: 2022
url: https://consensus.app/papers/details/e7769d1c1ed35e7380bdf4f8128dbd37/?utm_source=claude_desktop
doi: 10.48550/arxiv.2202.08906
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [Zoph 2022, router z-loss paper]
---

# ST-MoE (2022)

*Full title: "ST-MoE: Designing Stable and Transferable Sparse Expert Models"* (~440 citations).

## Summary

The MoE **stability & fine-tuning design guide**, attacking the two adoption blockers of the Switch era: training instabilities and poor fine-tuning quality. Its lasting contribution: the **router z-loss** — a small penalty on the log-sum-exp of router logits that keeps them from growing large, where bf16 round-off in the routing softmax destabilizes training. Distinct from the *balancing* loss: z-loss is about **numerics**, balancing loss about **load**; modern stacks (PaLM-MoE lineage, Qwen-MoE) carry both.

## Key results

- **ST-MoE-32B**: 269B sparse parameters at the compute cost of a 32B dense model — **first sparse model to reach SOTA in transfer learning** (SuperGLUE, ARC, XSum, CNN-DM, closed-book QA, ANLI)
- Systematic sweeps of which stability fixes hurt quality (many do) — z-loss stabilizes *without* the quality tax
- Diagnosed the sparse fine-tuning gap (overfitting dynamics differ; expert dropout guidance) documented earlier by [[Efficient Large Scale Language Modeling with Mixtures of Experts (2021)|Artetxe 2021]]

## Concepts extracted

- [[Mixture of Experts]] — stability (z-loss) in the balancing lineage
