---
type: source
source-type: paper
authors: [Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Łukasz Kaiser, Illia Polosukhin]
year: 2017
url: https://consensus.app/papers/details/948b6cae9eab51d59363e00237be52cd/?utm_source=claude_desktop
doi: 10.48550/arxiv.1706.03762
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-24
aliases: [Vaswani 2017, the Transformer paper]
---

# Attention Is All You Need (2017)

**The Transformer paper** (~189,000 citations — among the most cited papers in computer science history).

## Summary

Removes recurrence and convolution entirely: an encoder-decoder built from stacked blocks of multi-head scaled dot-product attention + position-wise FFN, with residual connections, layer norm, and sinusoidal positional encodings. Fully parallelizable over sequence positions.

## Key results

- **WMT'14 En→De: 28.4 BLEU** (beat all prior single models and ensembles by >2 BLEU); **En→Fr: 41.8 BLEU** single-model SOTA
- Trained in **3.5 days on 8 GPUs** — a fraction of the compute of the previous SOTA (parallelism was the point)
- Generalized beyond MT (constituency parsing) with minimal changes

## FFN specifics (why this vault cites it)

Set the conventions this vault's [[Feedforward Network]] note is about: $\text{FFN}(x) = \max(0, xW_1+b_1)W_2 + b_2$ — **dense, ReLU, $d_{ff} = 4d$** ($d=512$, $d_{ff}=2048$ base; $d=1024$, $d_{ff}=4096$ big). The 4× ratio was set here without ablation and held for ~6 years.

## Concepts extracted

- [[Transformer]], [[Attention Mechanism]], [[Multi-Head Attention]], [[Feedforward Network]], [[Positional Encoding]], [[Normalization]], [[Residual Connection]]
