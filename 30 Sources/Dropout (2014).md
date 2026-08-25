---
type: source
source-type: paper
authors: [Nitish Srivastava, Geoffrey Hinton, Alex Krizhevsky, Ilya Sutskever, Ruslan Salakhutdinov]
year: 2014
url: https://consensus.app/papers/details/e661433879ab59ce8cd92e286f289048/?utm_source=claude_desktop
doi: 10.5555/2627435.2670313
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-25
aliases: [Srivastava 2014, dropout paper]
---

# Dropout (2014)

*Full title: "Dropout: a simple way to prevent neural networks from overfitting"* (JMLR, ~44,000 citations).

## Summary

During training, randomly zero each unit with probability $p$; at test time use the full network with scaled weights. Interpretations: prevents co-adaptation of features, and approximates averaging an exponential ensemble of thinned sub-networks with one network.

## Key results

- Major overfitting reduction and SOTA-at-the-time across vision, speech, document classification, computational biology

## Modern status

Central to the AlexNet-to-BERT era; **largely absent from LLM pre-training** (one-epoch training on massive data rarely overfits — regularization shifted to weight decay + data scale), but alive in fine-tuning and small-data regimes. Its input-adaptive expectation is the derivation of [[Gaussian Error Linear Units (2016)|GELU]]; MoE expert dropout and z-loss-era tricks are descendants.

## Concepts extracted

- [[Neural Network]] — regularization
