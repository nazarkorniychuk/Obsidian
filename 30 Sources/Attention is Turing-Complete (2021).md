---
type: source
source-type: paper
authors: [Jorge Pérez, Pablo Barceló, Javier Marinkovic]
year: 2021
url: https://consensus.app/papers/details/a701abdd19b058c0b56498d76528adcb/?utm_source=claude_desktop
topics: [deep-learning]
status: processed
rating: 3
created: 2026-08-24
aliases: [Pérez 2021]
---

# Attention is Turing-Complete (2021)

(JMLR, ~230 citations.)

## Summary

Proves the transformer (encoder-decoder, hard attention) is **Turing complete** — it can simulate any Turing machine — *purely through its ability to compute and access internal dense representations*, with no external memory tape.

## The fine print (why this doesn't contradict [[Theoretical Limitations of Self-Attention (2020)|Hahn]])

The construction requires **arbitrary-precision rational activations** and unbounded decoding steps. Real transformers have fixed precision and fixed depth — under those constraints the negative results (PARITY, Dyck; saturated transformers ⊆ TC⁰ circuits per Merrill 2021) apply. Upper bound in the idealized regime, lower bounds in the realistic one.

## Concepts extracted

- [[Attention Mechanism]] — theory section, the optimistic bookend
