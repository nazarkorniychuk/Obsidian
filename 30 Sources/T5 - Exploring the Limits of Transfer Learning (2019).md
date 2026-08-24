---
type: source
source-type: paper
authors: [Colin Raffel, Noam Shazeer, Adam Roberts, Katherine Lee, Sharan Narang, Michael Matena, Yanqi Zhou, Wei Li, Peter J. Liu]
year: 2019
url: https://consensus.app/papers/details/5fc8d8f1be045829a1676c76833b9607/?utm_source=claude_desktop
doi: 10.48550/arxiv.1910.10683
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-24
aliases: [T5 paper, Raffel 2019]
---

# T5 - Exploring the Limits of Transfer Learning (2019)

*Full title: "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer"* (JMLR, ~27,000 citations).

## Summary

Casts every NLP task as text-to-text and runs the era's most systematic ablation study of pre-training (objectives, architectures, corpora — introducing C4), then scales to 11B parameters for SOTA on many benchmarks.

## FFN specifics (why this vault cites it)

- T5 kept the [[Attention Is All You Need (2017)|Vaswani]] **dense ReLU FFN** as its baseline — the testbed [[GLU Variants Improve Transformer (2020)|Shazeer 2020]] later ran the SwiGLU comparison on
- **T5-11B is the expansion-ratio outlier**: $d_{model}=1024$, $d_{ff}=65536$ → **64×** — scaling by inflating the FFN width instead of depth/embedding, an experiment nobody repeated
- T5-Base is the reference model that [[Switch Transformers (2021)]] beat 7× with MoE

## Concepts extracted

- [[Feedforward Network]] — the 64× outlier; SwiGLU/MoE baseline
- [[Transformer]]
