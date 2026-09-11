---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [Banach space, bounded operator, Hahn-Banach, open mapping theorem, closed graph theorem, uniform boundedness]
---

# Normed and Banach Spaces

> **Where this sits.** Chapter 2 of [[18.102 Functional Analysis]] — the home objects. A norm welds [[Topological and Metric Spaces|metric structure]] onto a vector space; **Banach = complete normed**. The chapter's three acts: what infinite dimensionality costs (no compact unit ball, discontinuous linear maps), Hahn–Banach (duals are rich), and the three Baire-powered **cornerstones**.

## The objects, and the dividing line

- **Norm**: homogeneous $\|\alpha v\| = |\alpha|\|v\|$, triangle inequality, definite ($\|v\|=0 \Rightarrow v=0$; drop definiteness → **seminorm**). Every normed space has a **completion**: a Banach space containing it isometrically as a dense subspace, unique up to isometry
- **Schauder basis** $\{e_n\}$: every $x$ is a *unique norm-convergent* series $\sum x_n e_n$ (≠ Hamel basis, which uses finite sums only). Schauder basis ⇒ separable; converse false
- **Riesz's lemma**: for any proper closed subspace $Y \subsetneq X$ there is a unit vector almost at distance 1 from $Y$ — "orthogonality up to ε" without an inner product
- **The dividing line of the subject**: $\overline{B}_1(0)$ **compact ⇔ finite-dimensional.** (⇐ Heine–Borel; ⇒ Riesz's lemma iterates unit vectors mutually ≥ ½ apart — no convergent subsequence.) Everything hard about infinite dimensions traces here; [[Dual Spaces and Weak Topologies|ch. 4]] is the project of buying compactness back with coarser topologies

## Linear operators: bounded = continuous

- $\|T\| = \sup_{x \neq 0} \|Tx\| / \|x\|$. **Bounded ⇔ continuous ⇔ continuous at one point** (linearity spreads local control globally). Finite-dimensional domain ⇒ automatically bounded — so discontinuous linear maps are a purely infinite-dimensional phenomenon
- The founding example pair, on $C[0,1]$ with sup norm: **integration is bounded** ($\|T_I\| = 1$), **differentiation is unbounded** (witness $\sin(nx)$: input norm ≤ 1, output norm ≥ n). The operators analysis most cares about are the *unbounded* ones — which is why domains $\operatorname{dom}(T) \subsetneq X$ are carried through the whole theory
- $BL(X,Y)$ with the operator norm is Banach **whenever $Y$ is** — in particular the dual $X^* = BL(X, \mathbb{K})$ is *always* Banach, complete or not $X$
- **Bounded linear extension**: a bounded operator on a dense subspace extends uniquely to the closure, same norm (how operators get defined on $L^p$: define on nice functions, extend)

## Hahn–Banach: duals are rich

**Theorem (real).** A linear functional on a subspace, dominated by a sublinear $p$, extends to the whole space still dominated by $p$. (Complex version: dominated by a seminorm in modulus.) *Mechanism:* extend one dimension at a time — the two constraints on the new value leave a nonempty interval — then Zorn to exhaust.

The corollaries are what get used:

- **Norming functionals exist**: for every $v \neq 0$ some $f \in X^*$ has $\|f\| = 1$, $f(v) = \|v\|$; hence $\|x\| = \sup_{\|f\|=1} |f(x)|$ — the dual *sees* the norm and **separates points** (contrast: TVS like $L^p$, $p<1$, with trivial dual)
- **Separation from closed subspaces**: $x \notin U$ closed ⇒ some $f \in X^*$ kills $U$ but not $x$ (via the quotient norm)
- **The canonical embedding** $i: X \to X^{**}$, $i(x)(f) = f(x)$, is an isometry. $X$ **reflexive** if it's onto. Scoreboard: finite-dim and $\ell^p, L^p$ ($1<p<\infty$) reflexive; $(c_0)^* = \ell^1$, $(\ell^1)^* = \ell^\infty$, and none of $c_0, \ell^1, \ell^\infty$ reflexive
- $X^*$ separable ⇒ $X$ separable (not conversely: $\ell^1$ separable, $(\ell^1)^* = \ell^\infty$ not)

## The three cornerstones — Baire cashed in

All three convert **completeness** into automatic regularity, via [[Topological and Metric Spaces#Baire category theorem — completeness as an engine|Baire]]: a Banach space can't be a countable union of nowhere dense sets, so *some* piece of any countable decomposition is somewhere dense — and linearity spreads that one dense patch everywhere.

1. **Uniform boundedness (Banach–Steinhaus).** $X$ Banach, $(T_n) \subset BL(X,Y)$: pointwise bounded ⇒ uniformly bounded, $\sup_n \|T_n\| < \infty$. *Mechanism:* Baire on $A_m = \{x : \sup_n \|T_n x\| \le m\}$. Reading: for operator families on complete spaces, qualitative boundedness upgrades to quantitative — for free
2. **Open mapping.** $X, Y$ Banach, $T$ bounded surjective ⇒ $T$ open. *Mechanism:* Baire finds an interior point of $\overline{T(B)}$; translation + scaling + an iterative approximation (completeness of $X$) removes the closure. Needs completeness of **both** spaces. Corollaries: **inverse mapping** (bounded bijection ⇒ bounded inverse — algebraic invertibility is automatically topological); injective $T$ has continuous inverse on its range ⇔ range closed
3. **Closed graph.** $T$ defined on all of a Banach space with closed graph ⇒ bounded. The point is the *weakening of what must be checked*: continuity = "(i) $x_n \to x$ ⇒ (ii) $Tx_n$ converges and (iii) to $Tx$"; closedness = "(i)+(ii) ⇒ (iii)" — you may *assume* $Tx_n$ converges. *Mechanism:* the graph is Banach; project to $X$; inverse mapping theorem. The example separating the notions: differentiation on $\operatorname{dom} = C^1 \subset C[0,1]$ is **closed but unbounded** — no contradiction, its domain isn't closed

## Related

- [[Topological and Metric Spaces]] — Baire, the engine under all three cornerstones
- [[Dual Spaces and Weak Topologies]] — what the rich dual is *for*
- [[Hilbert Spaces]] — the special case where the dual is the space itself
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 2

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
