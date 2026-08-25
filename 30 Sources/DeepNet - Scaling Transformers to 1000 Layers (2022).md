---
type: source
source-type: paper
authors: [Hongyu Wang, Shuming Ma, Li Dong, Shaohan Huang, Dongdong Zhang, Furu Wei]
year: 2022
url: https://consensus.app/papers/details/81b673b5cca85a9581d6a1bb789a32b3/?utm_source=claude_desktop
doi: 10.1109/tpami.2024.3386927
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-25
aliases: [DeepNorm paper, Wang 2022]
---

# DeepNet - Scaling Transformers to 1000 Layers (2022)

(~270 citations.)

## Summary

Instead of switching to pre-LN, **fix post-LN**: **DeepNorm** modifies the residual connection to $\text{LN}(\alpha \cdot x + f(x))$ with theoretically derived constants $\alpha, \beta$ (used to scale initialization), bounding model updates so training stays stable at extreme depth — keeping post-LN's performance with pre-LN's stability.

## Key results

- **1,000-layer transformers** (2,500 sublayers) trained without difficulty — an order of magnitude deeper than prior work
- 200-layer/3.2B model beats a 48-layer/12B SOTA by **5 BLEU** on a 7,482-direction multilingual benchmark — at extreme multilinguality, *depth beats width at ¼ the parameters*

## Concepts extracted

- [[Layer Normalization]], [[Residual Connection]] — the post-LN rescue; depth-vs-width evidence
