---
type: source
source-type: paper
authors: [Vinod Nair, Geoffrey Hinton]
year: 2010
url: https://consensus.app/papers/details/98b981b0bd8b5df3aa0ea9d31509d2fa/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-24
aliases: [Nair & Hinton 2010]
---

# Rectified Linear Units Improve Restricted Boltzmann Machines (2010)

**The ReLU origin paper** for deep learning (ICML 2010, ~19,000 citations) — the correct citation that [[Deep Learning using Rectified Linear Units (2018)]] restored to the record.

## Summary

Derives ReLU as the limit of stacking infinitely many binary stochastic units with shared weights and shifted biases ("stepped sigmoid units"), approximated efficiently by noisy rectified linear units.

## Key results

- ReLU features beat binary units on NORB object recognition and LFW face verification
- Unlike bounded units, ReLU **preserves relative intensity information** across layers — the property that later made deep gradient flow work

## Impact

With Glorot et al.'s [[Deep Sparse Rectifier Neural Networks (2011)]], set up ReLU as the deep-learning default that AlexNet (2012) then made universal.

## Concepts extracted

- [[Activation Function]] — ReLU origin
