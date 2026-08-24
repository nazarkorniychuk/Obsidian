---
type: source
source-type: paper
authors: [Christodoulos Peltekis, et al.]
year: 2024
url: https://consensus.app/papers/details/96182912c7c7521dadd536574cb2d716/?utm_source=claude_desktop
doi: 10.1109/aicas59952.2024.10595882
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Peltekis 2024]
---

# Reusing Softmax Hardware Unit for GELU (2024)

*Full title: "Reusing Softmax Hardware Unit for GELU Computation in Transformers."*

## Summary

Transformer accelerators must implement two non-linear functions in hardware: softmax (attention) and GELU (FFN). Normally each gets its own circuit. This paper maps GELU computation onto the **already-present softmax unit**, computing multiple GELU outputs in parallel through the vectorized softmax datapath.

## Key results

- **No accuracy loss** on representative NLP workloads
- **−6.1% total hardware area, −11.9% power** on average vs separate units

## Why it's in the vault

Concrete instance of the accelerator-design principle: transcendental activations (erf, exp) are the expensive minority of transformer silicon — so they get approximated, shared, or reused rather than paid for twice.

## Concepts extracted

- [[Activation Function]] — hardware section
