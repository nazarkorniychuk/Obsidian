---
type: source
source-type: paper
authors: [Djork-Arné Clevert, Thomas Unterthiner, Sepp Hochreiter]
year: 2015
url: https://consensus.app/papers/details/046aee25d6995601b4bb4d619c97fa2d/?utm_source=claude_desktop
doi: 10.48550/arxiv.1511.07289
topics: [deep-learning]
status: processed
rating: 4
created: 2026-08-23
aliases: [ELU paper, Clevert 2015]
---

# Fast and Accurate Deep Network Learning by Exponential Linear Units (2015)

**The ELU paper** (from Hochreiter's group — the vanishing-gradient/LSTM lineage).

## Summary

Proposes $\text{ELU}(x) = x$ for $x>0$, $\alpha(e^x - 1)$ for $x \le 0$. Keeps ReLU's identity-for-positives (no vanishing gradient) but replaces the hard zero with a smooth curve that **saturates to $-\alpha$**. Two motivations: (1) negative outputs push *mean activations toward zero*, reducing "bias shift" between layers — batch normalization's centering effect for free; (2) the negative saturation makes the off-state noise-robust: the unit codes *presence* of a feature precisely but doesn't quantify absence.

## Key results

- Faster learning and better generalization than ReLU/Leaky ReLU on networks **≥ 5 layers**
- On CIFAR-100: **ELU networks beat ReLU + batch norm**, and batch norm didn't help ELU networks further
- Top-10 CIFAR-10 result at the time; considerable ImageNet speedup at equal architecture
- ~6,200 citations

## Impact / adoption

Modest in practice — ELU never displaced ReLU (exp is costlier, gains shrank with better normalization). Its intellectual descendant SELU (self-normalizing networks) had a moment in 2017. The zero-mean argument survives in modern activation design.

## Concepts extracted

- [[Activation Function]] — ELU section, "repair family"
