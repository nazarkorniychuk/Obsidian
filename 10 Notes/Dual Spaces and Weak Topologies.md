---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [weak topology, weak-star topology, Banach-Alaoglu, Lp duality, weak convergence]
---

# Dual Spaces and Weak Topologies

> **Where this sits.** Chapter 4 of [[18.102 Functional Analysis]] — the compactness rescue. [[Normed and Banach Spaces|Ch. 2's dividing line]]: in infinite dimensions the unit ball is *never* norm-compact, so minimizing sequences have no convergent subsequences and existence proofs die. The fix: **make the topology coarser** — fewer open sets, more compact sets — exactly as coarse as the dual allows. Payoff: Banach–Alaoglu, and the explicit duals of $L^p$.

## The topologies

Machinery: a **locally convex space** = TVS whose topology comes from a separating family of seminorms. Both weak topologies are instances, and both are [[Topological and Metric Spaces|initial topologies]] ("coarsest making a family of maps continuous"):

| topology | on | induced by | convergence means |
|---|---|---|---|
| **weak** $\sigma(X, X^*)$ | $X$ | seminorms $x \mapsto \lvert f(x)\rvert$, $f \in X^*$ | $x_n \rightharpoonup x$ ⇔ $f(x_n) \to f(x)\ \forall f \in X^*$ |
| **weak-*** $\sigma(X^*, X)$ | $X^*$ | seminorms $f \mapsto \lvert f(x)\rvert$, $x \in X$ | $f_n \stackrel{*}{\rightharpoonup} f$ ⇔ $f_n(x) \to f(x)\ \forall x \in X$ |

- Weak ⊂ norm topology (agree iff finite-dimensional). Weak-* ⊂ weak-topology-of-$X^*$, equal iff $X$ reflexive — weak-* tests only against $X$, not all of $X^{**}$: it is the *cheapest* topology on the dual, which is exactly why its compactness theorem is strongest
- In infinite dimensions neither topology is metrizable on the whole space (though on *bounded balls* weak-* is, when $X$ is separable — the loophole below)
- Facts about weak convergence: weakly convergent ⇒ **bounded** (uniform boundedness in disguise) and the norm is weakly lower semicontinuous, $\|x\| \le \liminf \|x_n\|$ — mass can leak in a weak limit, never appear
- Curiosity with content: $\ell^1$ has the **Schur property** — weak and norm convergence of *sequences* coincide (the dual $\ell^\infty$ is that large) — yet the topologies still differ

## Banach–Alaoglu — compactness regained

**Theorem.** The closed unit ball of $X^*$ is **weak-\* compact.** *Mechanism, worth remembering:* a functional with $\|f\| \le 1$ is a point of the product $\prod_{x \in X} \{|\lambda| \le \|x\|\}$ — compact by [[Topological and Metric Spaces|Tychonoff]]; weak-* topology = product topology restricted there; "being linear" is a closed condition. So the ball is a closed subset of a compact product. □

**Separable case** ($X$ separable): the ball is weak-* **sequentially** compact — every bounded sequence of functionals has a weak-*-convergent subsequence (metrizability of the ball, or directly: diagonal argument over a countable dense set + bounded linear extension).

Why this is *the* existence tool: bounded sequence → weak-* cluster point, for free. In reflexive spaces the same works for the weak topology on $X$ itself — bounded sequences in $L^p$ ($1<p<\infty$) or [[Hilbert Spaces|Hilbert space]] have weakly convergent subsequences. This is the engine of the **direct method of the calculus of variations**: minimize by taking a bounded minimizing sequence, extract a weak limit, use lower semicontinuity. Caution: only the *ball* is compact — the ball has empty weak-* interior; the space is not locally compact.

## Riesz representation for $L^p$ — the duals, explicitly

With $\tfrac1p + \tfrac1q = 1$, the pairing $\phi_g(f) = \int g f \, d\mu$ gives:

$$\boxed{\;(L^p)^* \cong L^q \text{ isometrically, } 1 < p < \infty; \qquad (L^1)^* \cong L^\infty \text{ ($\sigma$-finite $\mu$)}\;}$$

- Hölder gives $\|\phi_g\| \le \|g\|_q$; equality by testing against $f = e^{i\theta}|g|^{q-1}$ (the equality case of Hölder, used as a *construction*)
- Surjectivity, this course's route (elementary, no Radon–Nikodym): given a norm-1 functional $F$, a norm-1 maximizing sequence is Cauchy **by uniform convexity** ([[Measure Theory and the Lebesgue Integral|Clarkson]]) — the maximizer $f$ exists, and $g \sim |f|^{p/q}$ represents $F$. $p = 1$ then follows by a limiting/gluing argument over finite-measure pieces
- Consequences: **$L^p$ reflexive for $1 < p < \infty$** (dual of the dual walks back). NOT reflexive: $\ell^1, \ell^\infty, L^1, L^\infty$. And the asymmetry to remember: $(L^1)^* = L^\infty$ but $(L^\infty)^* \supsetneq L^1$ — e.g. extend $f \mapsto f(0)$ from $C_b$ by Hahn–Banach: a functional on $L^\infty$ that no $L^1$ function represents (a "delta function" living in the bidual)

## Related

- [[Normed and Banach Spaces]] — duals, reflexivity, Hahn–Banach (used twice here)
- [[Measure Theory and the Lebesgue Integral]] — $L^p$, uniform convexity
- [[Hilbert Spaces]] — where the dual is the space itself and weak compactness is cleanest
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 4

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
