---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [topology, metric space, Baire category theorem, completeness]
---

# Topological and Metric Spaces

> **Where this sits.** Chapter 1 of [[18.102 Functional Analysis]] — the substrate. Three layers: the *language* (topology: openness, convergence, continuity without distances), the *working setting* (metric spaces: where sequences suffice), and the chapter's one deep theorem (**Baire**: completeness converted into a proof engine). Plus Zorn's lemma, the existence tool the whole course leans on. Format of every entry: *Idea* (words) → *Formally* (rigor) → *Mechanism* (why it's true), so each result can be read at either altitude.

## Zorn's lemma — the existence axiom

**Poset, chain, maximal element.** *Idea:* a set where some pairs are comparable ("bigger/smaller") but not necessarily all; a chain is a subset where everything *is* comparable; a maximal element is one with nothing strictly above it — not necessarily a greatest one. *Formally:* $\le$ on $P$ with $a \le a$; $a \le b \wedge b \le c \Rightarrow a \le c$; $a \le b \wedge b \le a \Rightarrow a = b$. Chain: totally ordered $C \subset P$. Maximal $x$: $x \le y \Rightarrow y = x$.

**Zorn's lemma (axiom).** *Idea:* if you can always top off any linearly ordered tower of partial constructions, then a construction exists that cannot be extended at all. *Formally:* $(P, \le)$ non-empty, every non-empty chain has an upper bound in $P$ ⇒ $P$ has a maximal element. Equivalent to the axiom of choice. *Usage pattern in this course:* the desired object (Hamel basis, Hahn–Banach extension) = a maximal element of $\{$partial versions, ordered by extension$\}$ — which is why Zorn-built objects are always highly non-unique.

**Hamel basis.** *Idea:* a basis in the linear-algebra sense — every vector is a *finite* combination — and every vector space has one, though for infinite dimensions you can never write it down. *Formally:* $B \subset V$ linearly independent with $\operatorname{span}(B) = V$; existence for arbitrary $V$ by Zorn on linearly independent sets.

## Topological spaces — the minimal language

**Topology.** *Idea:* forget distances; just declare which sets count as "open," subject to the closure rules unions and finite intersections obey in $\mathbb{R}^n$. Everything else — convergence, continuity, compactness — is *derived* from the declaration. *Formally:* $\mathcal{T} \subset \mathcal{P}(X)$ with $\emptyset, X \in \mathcal{T}$; $\bigcup_{i \in I} U_i \in \mathcal{T}$ for arbitrary $I$; $\bigcap_{i=1}^{n} U_i \in \mathcal{T}$. $A$ closed $:\Leftrightarrow A^c \in \mathcal{T}$.

**Hausdorff.** *Idea:* any two points can be housed in disjoint open bubbles — the minimal decency condition that makes limits unique. *Formally:* $\forall x \neq y\ \exists$ open $U_x \ni x, U_y \ni y$ with $U_x \cap U_y = \emptyset$.

**Interior, closure, dense.** *Idea:* the interior is what's robustly inside $A$; the closure adds every point $A$ crowds against; dense means "crowds against everything." *Formally:* $\operatorname{int}(A)$ = largest open $\subset A$; $\overline{A}$ = smallest closed $\supset A$; $A$ dense $:\Leftrightarrow \overline{A} = X$. Convergence: $x_n \to x :\Leftrightarrow$ every neighborhood of $x$ contains a tail of the sequence.

**Continuity, and its sequential shadow.** *Idea:* continuous = pulling back open sets gives open sets; sequentially continuous = respects limits of sequences. These agree only when the space is "small enough at each point" for sequences to detect the topology. *Formally:* $f$ continuous $:\Leftrightarrow f^{-1}(U) \in \mathcal{T}_X\ \forall U \in \mathcal{T}_Y$; continuous ⇒ sequentially continuous, converse holds if $X$ is **first countable** (countable neighborhood base at each point).

**Initial topology.** *Idea:* given maps out of $X$, install on $X$ the *stingiest* topology that still makes them all continuous — no open set exists without a reason. File this construction: [[Dual Spaces and Weak Topologies|weak and weak-* topologies]] are exactly this, and so is the product topology (maps = coordinate projections). *Formally:* the coarsest $\mathcal{T}$ on $X$ making every $f_i: X \to X_i$ continuous; subbase $\{f_i^{-1}(U) : U \in \mathcal{T}_i\}$.

**Countability ladder.** *Formally:* second countable (countable base) ⇒ first countable + separable ($\exists$ countable dense set). In metric spaces separable ⇔ second countable.

**Compactness.** *Idea:* a compact set is one where no infinite process escapes — every attempt to cover it with open sets already succeeds with finitely many. It is the property that turns "for each point, locally…" into "globally, uniformly…". *Formally:* every open cover of $K$ has a finite subcover ⇔ every family of closed subsets with the finite intersection property has $\bigcap \neq \emptyset$.

**Tychonoff.** *Idea:* compactness survives *arbitrary* products — the single most powerful compactness generator, and the muscle inside [[Dual Spaces and Weak Topologies|Banach–Alaoglu]]. *Formally:* $\prod_{i \in I} X_i$ compact (product topology) ⇔ each $X_i$ compact. *Mechanism:* Zorn gives a maximal finite-intersection family; compactness of each factor picks a coordinate-wise limit point.

**Topological vector space.** *Idea:* a vector space whose topology and algebra cooperate — addition and scaling are continuous. *Formally:* Hausdorff $\mathcal{T}$ with $+: V \times V \to V$, $\cdot: \mathbb{K} \times V \to V$ continuous. Cautionary example: $L^p$ with $0 < p < 1$ is a TVS whose continuous dual is $\{0\}$ — the disease [[Normed and Banach Spaces|Hahn–Banach]] rules out for normed spaces.

## Metric spaces — where sequences suffice

**Metric.** *Idea:* an abstract distance: zero only for identical points, symmetric, no shortcuts through a third point. *Formally:* $d: X \times X \to [0,\infty)$ with $d(x,y) = 0 \Leftrightarrow x = y$; $d(x,y) = d(y,x)$; $d(x,y) \le d(x,z) + d(z,y)$. Balls $B_\varepsilon(x) = \{y: d(x,y) < \varepsilon\}$ generate a topology that is Hausdorff **and first countable — so in metric spaces, sequences detect everything** (closedness, continuity, compactness).

**Completeness.** *Idea:* every sequence that "internally settles down" actually lands somewhere — the space has no missing points. *Formally:* every Cauchy sequence ($\forall \varepsilon\ \exists N: d(x_n, x_m) < \varepsilon\ \forall n,m \ge N$) converges in $X$. Two standing warnings: completeness is a property of the *metric*, not the topology; and for $X$ complete, $W \subset X$ is complete ⇔ closed.

**Completion.** *Idea:* every metric space can have its missing points glued in, essentially uniquely — the abstract template for building $L^p$ before measure theory does it concretely. *Formally:* $\forall (X,d)\ \exists$ complete $(X', d')$ + isometry $\Phi: X \to X'$ with $\overline{\Phi(X)} = X'$; unique up to isometry. *Mechanism:* points of $X'$ = Cauchy sequences modulo $(x_n) \sim (y_n) :\Leftrightarrow d(x_n, y_n) \to 0$.

**Compactness in metric spaces.** *Formally:* compact ⇔ sequentially compact (every sequence has a convergent subsequence).

**The inequality chain.** *Idea:* one convexity fact about logarithms cascades into the triangle inequality for every $p$-norm. Conjugate exponents $\tfrac1p + \tfrac1q = 1$ are *forced*, not chosen. *Formally:*

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

**Small and large, topologically.** *Idea:* "nowhere dense" = so sparse that even its closure has no interior — thin everywhere; "meager" = coverable by countably many such sets — negligible in the Baire sense; "residual" = complement of meager — the topological "almost all." *Formally:* $A$ nowhere dense $:\Leftrightarrow \operatorname{int}(\overline{A}) = \emptyset$; meager $:\Leftrightarrow A \subset \bigcup_{n} B_n$, $B_n$ nowhere dense; residual $:\Leftrightarrow A^c$ meager.

**Baire category theorem.** *Idea:* a complete metric space is never negligible in its own eyes — you cannot exhaust it by countably many thin sets. Equivalently: countably many dense open layers still intersect densely. *Formally:* $X$ complete metric, $A_n$ open and dense for all $n \in \mathbb{N}$ ⇒ $\bigcap_{n \in \mathbb{N}} A_n$ dense; equivalently, $X$ complete ⇒ $X$ is not meager in itself. *Mechanism:* inside any ball, nest shrinking closed balls $\overline{B}_{\varepsilon_n}(x_n) \subset B_{\varepsilon_{n-1}}(x_{n-1}) \cap A_n$ with $\varepsilon_n < \tfrac1n$; the centers are Cauchy; completeness delivers a limit lying in every $A_n$.

Two reasons it's here:

1. **It powers ch. 2's three cornerstones** ([[Normed and Banach Spaces|uniform boundedness, open mapping, closed graph]]) — each is Baire applied to a cleverly chosen countable decomposition $X = \bigcup_m A_m$: some $A_m$ must be somewhere dense, and linearity spreads that patch everywhere
2. **Genericity arguments**: $\{f \in C[0,1] : f \text{ nowhere differentiable}\}$ is residual — Weierstraß's "monster" is the *typical* continuous function, not the exception; the template for all "most objects are wild" proofs

## Related

- [[Normed and Banach Spaces]] — the next layer of structure; Baire's payoff
- [[Dual Spaces and Weak Topologies]] — initial topologies and Tychonoff, cashed in
- [[Functional Analysis Definitions]] — the glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 1

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
