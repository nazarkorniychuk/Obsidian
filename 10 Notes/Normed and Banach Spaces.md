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

- **Norm**: $\|\alpha v\| = |\alpha| \|v\|$, $\|v + w\| \le \|v\| + \|w\|$, $\|v\| = 0 \Rightarrow v = 0$ (drop the last → **seminorm**); metric $d(v,w) = \|v - w\|$. Every normed space has a **completion**: a Banach space containing it isometrically as a dense subspace, unique up to isometric isomorphism
- **Schauder basis** $\{e_n\}$: $\forall x\ \exists!\,(x_n) \subset \mathbb{K}$ with $\big\|x - \sum_{n=1}^{N} x_n e_n\big\| \xrightarrow{N \to \infty} 0$ (≠ Hamel basis: norm-convergent series vs finite sums). Schauder basis ⇒ separable; converse false
- **Riesz's lemma**: $Y \subsetneq X$ closed subspace, $\varepsilon > 0$ ⇒ $\exists x,\ \|x\| = 1,\ \operatorname{dist}(x, Y) \ge 1 - \varepsilon$ — "orthogonality up to ε" without an inner product
- **The dividing line of the subject**:
$$\overline{B}_1(0) \text{ compact} \iff \dim X < \infty$$
  (⇐ Heine–Borel; ⇒ Riesz's lemma iterates unit vectors with mutual distance $\ge \tfrac12$ — no convergent subsequence.) Everything hard about infinite dimensions traces here; [[Dual Spaces and Weak Topologies|ch. 4]] is the project of buying compactness back with coarser topologies

## Linear operators: bounded = continuous

- $\|T\| = \sup_{x \neq 0} \frac{\|Tx\|_W}{\|x\|_V}$; **bounded ($\|T\| < \infty$) ⇔ continuous ⇔ continuous at one point** (linearity spreads local control globally). $\dim V < \infty$ ⇒ every linear $T$ bounded — discontinuous linear maps are a purely infinite-dimensional phenomenon
- The founding example pair on $(C[0,1], \|\cdot\|_\infty)$: **integration** $T_I f = \int_0^x f$ is bounded, $\|T_I\| = 1$; **differentiation** $T_D f = f'$ on $\operatorname{dom} = C^1$ is unbounded: $f_n = \sin(nx)$ gives $\|f_n\|_\infty \le 1$, $\|f_n'\|_\infty \ge n$. The operators analysis most cares about are the *unbounded* ones — hence domains $\operatorname{dom}(T) \subsetneq X$ are carried through the whole theory
- $BL(X,Y)$ with the operator norm is Banach **whenever $Y$ is** ⇒ the dual $X^* = BL(X, \mathbb{K})$ is *always* Banach, complete or not $X$
- **Bounded linear extension**: $T$ bounded on $\operatorname{dom}(T)$ dense, $Y$ Banach ⇒ $\exists!\ \tilde{T}: X \to Y$ with $\tilde{T}|_{\operatorname{dom}(T)} = T$, $\|\tilde{T}\| = \|T\|$ (how operators get defined on $L^p$: define on nice functions, extend)

## Hahn–Banach: duals are rich

**Theorem (real).** $p: V \to \mathbb{R}$ sublinear, $W \subset V$ subspace, $f: W \to \mathbb{R}$ linear with $f \le p$ on $W$ ⇒ $\exists f^*: V \to \mathbb{R}$ linear, $f^*|_W = f$, $f^* \le p$ on all of $V$. (Complex version: $|f| \le p$ for a seminorm $p$.) *Mechanism:* extend one dimension at a time — the constraints $\sup_y (f(y) - p(y - v)) \le f^*(v) \le \inf_x (p(x + v) - f(x))$ leave a nonempty interval — then Zorn to exhaust.

The corollaries are what get used:

- **Norming functionals exist**: $\forall v \neq 0\ \exists f \in X^*: \|f\| = 1,\ f(v) = \|v\|$; hence
$$\|x\| = \sup_{f \in X^*,\, \|f\| = 1} |f(x)|$$
  — the dual *sees* the norm and **separates points** (contrast: TVS like $L^p$, $p<1$, with $X^* = \{0\}$)
- **Separation from closed subspaces**: $U \subsetneq X$ closed, $x \notin U$ ⇒ $\exists f \in X^*: f|_U = 0,\ f(x) \neq 0$ (via the quotient norm $\|[v]\| = \operatorname{dist}(v, U)$)
- **The canonical embedding** $i: X \to X^{**}$, $i(x)(f) = f(x)$, is an isometry ($\|i(x)\| = \|x\|$, by the sup formula). $X$ **reflexive** $:\Leftrightarrow i$ surjective. Scoreboard: finite-dim and $\ell^p, L^p$ ($1<p<\infty$) reflexive; $(c_0)^* \cong \ell^1$, $(\ell^1)^* \cong \ell^\infty$, and none of $c_0, \ell^1, \ell^\infty$ reflexive
- $X^*$ separable ⇒ $X$ separable (not conversely: $\ell^1$ separable, $(\ell^1)^* = \ell^\infty$ not)

## The three cornerstones — Baire cashed in

All three convert **completeness** into automatic regularity, via [[Topological and Metric Spaces#Baire category theorem — completeness as an engine|Baire]]: a Banach space is not $\bigcup_{m \in \mathbb{N}} (\text{nowhere dense})$, so *some* member of any countable decomposition is somewhere dense — and linearity spreads that one dense patch everywhere.

1. **Uniform boundedness (Banach–Steinhaus).** $X$ Banach, $(T_n) \subset BL(X,Y)$:
$$\sup_n \|T_n x\| < \infty \ \ \forall x \in X \quad\Longrightarrow\quad \sup_n \|T_n\| < \infty$$
   *Mechanism:* Baire on $A_m = \{x : \sup_n \|T_n x\| \le m\}$, $X = \bigcup_m A_m$. Reading: for operator families on complete spaces, qualitative (pointwise) boundedness upgrades to quantitative (uniform) — for free
2. **Open mapping.** $X, Y$ Banach, $T \in BL(X,Y)$ **surjective** ⇒ $T$ open (i.e. $\exists \varepsilon: B_\varepsilon^Y(0) \subset T(B_1^X(0))$). *Mechanism:* Baire on $Y = \bigcup_n \overline{T(B_n(0))}$ finds an interior point; translation + scaling + iterative approximation (completeness of $X$) removes the closure. Needs completeness of **both** spaces. Corollaries: **inverse mapping** ($T$ bounded bijection ⇒ $T^{-1} \in BL(Y,X)$ — algebraic invertibility is automatically topological); for injective $T$: $T^{-1}$ continuous on $\operatorname{range}(T)$ ⇔ $\operatorname{range}(T)$ closed
3. **Closed graph.** $T$ linear with $\operatorname{dom}(T) = X$ Banach and $\operatorname{graph}(T) = \{(x, Tx)\}$ closed ⇒ $T$ bounded. The point is *what must be checked shrinks*: continuity = "$x_n \to x$ ⇒ $Tx_n$ converges *and* $\lim Tx_n = Tx$"; closedness = "$x_n \to x$ **and** $Tx_n \to y$ ⇒ $y = Tx$" — convergence of $Tx_n$ may be *assumed*. *Mechanism:* $\operatorname{graph}(T)$ is Banach; $\pi_X: \operatorname{graph}(T) \to X$ is a bounded bijection; inverse mapping theorem. The separating example: $T_D$ on $\operatorname{dom} = C^1 \subset C[0,1]$ is **closed but unbounded** — no contradiction, $C^1$ is not closed in $C[0,1]$

## Related

- [[Topological and Metric Spaces]] — Baire, the engine under all three cornerstones
- [[Dual Spaces and Weak Topologies]] — what the rich dual is *for*
- [[Hilbert Spaces]] — the special case where the dual is the space itself
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 2

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
