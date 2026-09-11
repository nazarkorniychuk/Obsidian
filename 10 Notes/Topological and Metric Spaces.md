---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [topology, metric space, Baire category theorem, completeness]
---

# Topological and Metric Spaces

> **Where this sits.** Chapter 1 of [[18.102 Functional Analysis]] — the substrate. Three layers: the *language* (topology: openness, convergence, continuity without distances), the *working setting* (metric spaces: where sequences suffice), and the chapter's one deep theorem (**Baire**: completeness converted into a proof engine). Plus Zorn's lemma, the existence tool the whole course leans on.

## Zorn's lemma — the existence axiom

$(P, \le)$ poset; $C \subset P$ a **chain** if totally ordered; $x$ **maximal** if $x \le y \Rightarrow x = y$. **Zorn:** *every non-empty poset in which every non-empty chain has an upper bound contains a maximal element.* Equivalent to the axiom of choice; treated as an axiom. Its job: produce **highly non-unique objects** — every vector space has a Hamel basis, Hahn–Banach extensions, Tychonoff. Pattern of use: the desired object = a maximal element of $\{$partial versions, ordered by extension$\}$.

## Topological spaces — the minimal language

- **Topology**: $\mathcal{T} \subset \mathcal{P}(X)$ with $\emptyset, X \in \mathcal{T}$; $\bigcup_{i \in I} U_i \in \mathcal{T}$ (arbitrary); $\bigcap_{i=1}^{n} U_i \in \mathcal{T}$ (finite). $A$ closed $:\Leftrightarrow A^c \in \mathcal{T}$. **Hausdorff**: $\forall x \neq y\ \exists U_x \ni x,\, U_y \ni y$ open, $U_x \cap U_y = \emptyset$ (⇒ limits unique)
- $\operatorname{int}(A)$ = largest open $\subset A$; $\overline{A}$ = smallest closed $\supset A$; $A$ **dense** $:\Leftrightarrow \overline{A} = X$; $x_n \to x :\Leftrightarrow$ every neighborhood of $x$ contains a tail
- **Continuity**: $f^{-1}(U)$ open $\forall U$ open. Continuous ⇒ sequentially continuous ($x_n \to x \Rightarrow f(x_n) \to f(x)$); converse requires **first countability** — the reason sequences don't suffice in general topologies but do in metric spaces
- **Initial topology** induced by $(f_i: X \to X_i)_{i \in I}$: the *coarsest* $\mathcal{T}$ making every $f_i$ continuous. File this — it is *the* construction behind the [[Dual Spaces and Weak Topologies|weak and weak-* topologies]] of ch. 4; the product topology on $\prod_i X_i$ is the case $f_i = \pi_i$
- Countability ladder: second countable (countable base) ⇒ first countable (countable neighborhood base $\forall x$) + separable ($\exists$ countable dense set)
- **Compact**: every open cover of $K$ has a finite subcover ⇔ every family of closed sets with the finite intersection property has $\bigcap \neq \emptyset$. **Tychonoff**: $\prod_{i \in I} X_i$ compact ⇔ each $X_i$ compact — arbitrary products, proved with Zorn; the muscle behind [[Dual Spaces and Weak Topologies|Banach–Alaoglu]]
- **Topological vector space**: Hausdorff $\mathcal{T}$ with $+: V \times V \to V$ and $\cdot: \mathbb{K} \times V \to V$ continuous. Warning example kept for motivation: $L^p$, $0<p<1$, is a TVS with $V^* = \{0\}$ — the disease [[Normed and Banach Spaces|Hahn–Banach]] rules out for normed spaces

## Metric spaces — where sequences suffice

- **Metric**: $d: X \times X \to [0,\infty)$, $d(x,y) = 0 \Leftrightarrow x = y$, $d(x,y) = d(y,x)$, $d(x,y) \le d(x,z) + d(z,y)$. The induced topology (balls $B_\varepsilon(x) = \{y : d(x,y) < \varepsilon\}$ as base) is Hausdorff + first countable ⇒ **everything is testable on sequences**
- **Complete**: every Cauchy sequence ($\forall \varepsilon\ \exists N: d(x_n, x_m) < \varepsilon\ \forall n,m \ge N$) converges. Two standing warnings: completeness is a *metric* property, not topological (one topology can carry a complete and an incomplete metric); $W \subset X$ complete ⇔ $W$ closed (for $X$ complete)
- **Completion**: $\forall (X,d)\ \exists$ complete $(X', d')$ and isometry $\Phi: X \to X'$ with $\overline{\Phi(X)} = X'$, unique up to isometry. Construction: Cauchy sequences mod $(x_n) \sim (y_n) :\Leftrightarrow d(x_n, y_n) \to 0$. (One of *two* routes to $L^p$ — the abstract one; ch. 3's measure theory is the concrete one)
- Metric specials: compact ⇔ sequentially compact; separable ⇔ second countable

**The inequality chain** (used everywhere; conjugate exponents $\tfrac1p + \tfrac1q = 1$ — *necessary*, not aesthetic):

$$\underbrace{ab \le \tfrac{a^p}{p} + \tfrac{b^q}{q}}_{\text{Young (concavity of } \log)} \;\Longrightarrow\; \underbrace{\textstyle\sum_n |x_n y_n| \le \|x\|_p \|y\|_q}_{\text{Hölder (normalize + termwise Young)}} \;\Longrightarrow\; \underbrace{\|x + y\|_p \le \|x\|_p + \|y\|_p}_{\text{Minkowski = triangle inequality for } \|\cdot\|_p}$$

**The example zoo** (the course's recurring test cases):

| space | definition | complete? | separable? |
|---|---|---|---|
| $\ell^p$, $1 \le p < \infty$ | $\sum_n \lvert x_n \rvert^p < \infty$, $\ \lVert x \rVert_p = (\sum_n \lvert x_n \rvert^p)^{1/p}$ | ✅ | ✅ |
| $\ell^\infty$ | $\lVert x \rVert_\infty = \sup_n \lvert x_n \rvert < \infty$ | ✅ | ❌ — the standard "too big" space |
| $c_0$ | $x_n \to 0$, sup norm | ✅ | ✅ |
| $c_c$ | finitely supported | ❌ — dense in $c_0$ | ✅ |
| $C(K)$, $K$ compact | $d(f,g) = \sup_K \lvert f - g \rvert$ | ✅ (uniform limit of continuous is continuous) | ✅ |

## Baire category theorem — completeness as an engine

Vocabulary: $A$ **nowhere dense** $:\Leftrightarrow \operatorname{int}(\overline{A}) = \emptyset$ → **meager** $:\Leftrightarrow A \subset \bigcup_{n \in \mathbb{N}} B_n$, $B_n$ nowhere dense ("small") → **residual / Baire-generic** $:\Leftrightarrow A^c$ meager ("almost all").

**Baire:** *$X$ complete metric, $A_n$ open and dense $\forall n \in \mathbb{N}$ $\Rightarrow$ $\bigcap_{n \in \mathbb{N}} A_n$ dense.* Equivalently: **a complete metric space is not a countable union of nowhere dense sets.** Proof mechanism: nest shrinking closed balls $\overline{B}_{\varepsilon_n}(x_n) \subset B_{\varepsilon_{n-1}}(x_{n-1}) \cap A_n$, $\varepsilon_n < \tfrac1n$; completeness delivers $\lim x_n \in \bigcap_n A_n$.

Two reasons it's here:

1. **It powers ch. 2's three cornerstones** ([[Normed and Banach Spaces|uniform boundedness, open mapping, closed graph]]) — each is Baire applied to a cleverly chosen countable decomposition $X = \bigcup_m A_m$. This is the course's main use
2. **Genericity arguments**: $\{f \in C[0,1] : f$ nowhere differentiable$\}$ is Baire-generic — Weierstraß's "monster" is the typical case, not the exception; the template for "most objects are wild" proofs

## Related

- [[Normed and Banach Spaces]] — the next layer of structure; Baire's payoff
- [[Dual Spaces and Weak Topologies]] — initial topologies and Tychonoff, cashed in
- [[Functional Analysis Definitions]] — the glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 1

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
