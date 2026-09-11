---
type: reference
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [FA definitions, functional analysis glossary]
---

# Functional Analysis Definitions

> The load-bearing definitions of [[18.102 Functional Analysis]], stated precisely, grouped by chapter — one term per entry. Convention throughout: $\mathbb{K} \in \{\mathbb{R}, \mathbb{C}\}$. For theorems and context, follow the chapter links.

## Ch. 1 — [[Topological and Metric Spaces]]

- **Partial order** on a set $P$: a relation $\le$ such that for all $a, b, c \in P$: $a \le a$; $a \le b \wedge b \le c \Rightarrow a \le c$; $a \le b \wedge b \le a \Rightarrow a = b$. The pair $(P, \le)$ is a **poset**
- **Chain**: a subset $C \subset P$ that is totally ordered — $x \le y$ or $y \le x$ for all $x, y \in C$
- **Upper bound** of $A \subset P$: an $x \in P$ with $a \le x$ for all $a \in A$. **Maximal element**: an $x$ such that $x \le y$ implies $y = x$
- **Zorn's lemma** (axiom): if $(P, \le)$ is non-empty and every non-empty chain has an upper bound in $P$, then $P$ has a maximal element
- **Hamel basis**: $B \subset V$ that is linearly independent (every finite subfamily) and spans $V$ via *finite* linear combinations. Every vector space has one (Zorn)
- **Topology** on $X$: a family $\mathcal{T} \subset \mathcal{P}(X)$ with (i) $\emptyset, X \in \mathcal{T}$; (ii) arbitrary unions: $U_i \in \mathcal{T},\ i \in I \Rightarrow \bigcup_{i \in I} U_i \in \mathcal{T}$; (iii) finite intersections: $U_1, \dots, U_n \in \mathcal{T} \Rightarrow \bigcap_{i=1}^n U_i \in \mathcal{T}$. Elements of $\mathcal{T}$ are the **open** sets; $A$ is **closed** iff $A^c \in \mathcal{T}$
- **Neighborhood** of $x$: any $A \subset X$ such that there is an open $U$ with $x \in U \subset A$
- **Hausdorff**: for all $x \neq y$ there are neighborhoods $U_x \ni x$ and $U_y \ni y$ with $U_x \cap U_y = \emptyset$. Guarantees limits are unique
- **Interior**: $\operatorname{int}(A)$ = the largest open set contained in $A$ — equivalently, the points that have a neighborhood inside $A$
- **Closure**: $\overline{A}$ = the smallest closed set containing $A$
- **Boundary**: $\partial A$ = the points whose every neighborhood meets both $A$ and $A^c$; $\overline{A} = A \cup \partial A$
- **Dense**: $A \subset X$ with $\overline{A} = X$
- **Convergence**: $x_n \to x$ iff for every neighborhood $U$ of $x$ there is $N$ with $x_n \in U$ for all $n \ge N$
- **Continuous** $f: X \to Y$: $f^{-1}(U)$ is open in $X$ for every open $U \subset Y$
- **Sequentially continuous**: $x_n \to x \Rightarrow f(x_n) \to f(x)$. Continuous ⇒ sequentially continuous; the converse holds when $X$ is first countable
- **Initial topology** induced by a family $(f_i: X \to X_i)_{i \in I}$: the coarsest topology on $X$ making every $f_i$ continuous
- **Product topology** on $\prod_{i \in I} X_i$: the initial topology of the coordinate projections $\pi_j$
- **Base**: $\mathcal{B} \subset \mathcal{T}$ such that every open set is a union of elements of $\mathcal{B}$. **Subbase**: a family whose finite intersections form a base
- **First countable**: every point has a countable neighborhood base. **Second countable**: $X$ has a countable base. **Separable**: $X$ contains a countable dense subset. (Second countable ⇒ both others)
- **Compact** $K$: every open cover of $K$ admits a finite subcover
- **Sequentially compact**: every sequence in $K$ has a subsequence converging in $K$. Equivalent to compactness in metric spaces
- **Relatively compact**: $\overline{K}$ is compact
- **Topological vector space (TVS)**: a vector space $V$ with a Hausdorff topology making addition $V \times V \to V$ and scalar multiplication $\mathbb{K} \times V \to V$ continuous
- **Metric** on $X$: $d: X \times X \to [0, \infty)$ with, for all $x, y, z$: $d(x,y) = 0 \Leftrightarrow x = y$; $d(x,y) = d(y,x)$; $d(x,y) \le d(x,z) + d(z,y)$. Open ball: $B_\varepsilon(x) = \{y : d(x,y) < \varepsilon\}$
- **Cauchy sequence**: for every $\varepsilon > 0$ there is $N$ with $d(x_n, x_m) < \varepsilon$ for all $n, m \ge N$
- **Complete**: every Cauchy sequence converges in $X$. A metric property, not a topological one
- **Isometry**: $\Phi: X \to Y$ with $d_Y(\Phi x, \Phi y) = d_X(x, y)$ for all $x, y$
- **Completion** of $(X, d)$: a complete space $(X', d')$ together with an isometry $\Phi: X \to X'$ whose image is dense; unique up to isometry
- **Hölder conjugates**: $p, q \in [1, \infty]$ with $\tfrac1p + \tfrac1q = 1$
- **Sequence spaces** over $\mathbb{K}$: $\ell^p = \{(x_n) : \sum_n |x_n|^p < \infty\}$ with $\|x\|_p = \big(\sum_n |x_n|^p\big)^{1/p}$ for $1 \le p < \infty$; $\ell^\infty = \{(x_n) : \sup_n |x_n| < \infty\}$ with $\|x\|_\infty = \sup_n |x_n|$; $c_0$ = the null sequences ($x_n \to 0$) in $\ell^\infty$; $c_c$ = the finitely supported sequences
- **Nowhere dense**: $\operatorname{int}(\overline{A}) = \emptyset$
- **Meager** (first category): $A \subset \bigcup_{n \in \mathbb{N}} B_n$ with each $B_n$ nowhere dense — "small" in the sense of Baire
- **Residual / Baire-generic**: $A^c$ is meager — "almost all" in the sense of Baire

## Ch. 2 — [[Normed and Banach Spaces]]

- **Norm** on $V$: $\|\cdot\|: V \to [0, \infty)$ with $\|\alpha v\| = |\alpha| \|v\|$; $\|v + w\| \le \|v\| + \|w\|$; $\|v\| = 0 \Rightarrow v = 0$. Induces the metric $d(v, w) = \|v - w\|$
- **Seminorm**: same, without positive definiteness — $\|v\| = 0$ is allowed for $v \neq 0$
- **Banach space**: a normed space complete in the induced metric
- **Schauder basis** $\{e_n\}_{n \in \mathbb{N}}$: for every $x \in X$ there is a *unique* scalar sequence $(x_n)$ with $\big\|x - \sum_{n=1}^N x_n e_n\big\| \to 0$ as $N \to \infty$. (Norm-convergent series — contrast the Hamel basis's finite sums)
- **Bounded operator**: a linear $T: \operatorname{dom}(T) \subset V \to W$ with
$$\|T\| = \sup_{0 \neq x \in \operatorname{dom}(T)} \frac{\|Tx\|_W}{\|x\|_V} < \infty$$
  Bounded ⇔ continuous. $BL(V, W)$ = the bounded linear maps $V \to W$, normed by $\|\cdot\|$
- **Dual space**: $X^* = BL(X, \mathbb{K})$ — the *continuous* linear functionals; always Banach. The **algebraic dual** $X'$ is all linear functionals, continuous or not
- **Sublinear functional**: $p: V \to \mathbb{R}$ with $p(x + y) \le p(x) + p(y)$ and $p(\lambda x) = \lambda p(x)$ for $\lambda \ge 0$ (Hahn–Banach's dominating gauge)
- **Bidual**: $X^{**} = (X^*)^*$
- **Canonical embedding**: $i: X \to X^{**}$, $i(x)(f) = f(x)$ — always a linear isometry (Hahn–Banach)
- **Reflexive**: $i$ is surjective, i.e. $X \cong X^{**}$ canonically
- **Closed operator**: $\operatorname{graph}(T) = \{(x, Tx) : x \in \operatorname{dom}(T)\}$ is closed in $V \times W$ with norm $\|(v,w)\| = \|v\| + \|w\|$. Sequential form: $x_n \to x$ and $T x_n \to y$ imply $x \in \operatorname{dom}(T)$ and $y = Tx$

## Ch. 3 — [[Measure Theory and the Lebesgue Integral]]

- **σ-algebra** on $\Omega$: $\mathcal{F} \subset \mathcal{P}(\Omega)$ with $\emptyset \in \mathcal{F}$; $A \in \mathcal{F} \Rightarrow A^c \in \mathcal{F}$; $A_i \in \mathcal{F}$ for $i \in \mathbb{N}$ $\Rightarrow \bigcup_i A_i \in \mathcal{F}$. Elements are the **measurable sets**
- **Generated σ-algebra** $\sigma(\mathcal{A})$: the smallest σ-algebra containing $\mathcal{A}$ (the intersection of all of them)
- **Measure**: $\mu: \mathcal{F} \to [0, \infty]$ with $\mu(\emptyset) = 0$ and countable additivity — $\mu\big(\bigsqcup_{i \in \mathbb{N}} A_i\big) = \sum_i \mu(A_i)$ for pairwise disjoint $A_i \in \mathcal{F}$
- **Null set**: $A \in \mathcal{F}$ with $\mu(A) = 0$. **Almost everywhere (a.e.)**: a property holding outside some null set
- **Finite / σ-finite**: $\mu(\Omega) < \infty$ / $\Omega = \bigcup_{n \in \mathbb{N}} A_n$ with every $\mu(A_n) < \infty$
- **Borel σ-algebra**: $\mathcal{B} = \sigma(\mathcal{T})$, generated by the open sets. A **Borel measure** is a measure on $\mathcal{B}$ that is finite on compact sets
- **Outer measure**: $\mu^*: \mathcal{P}(\Omega) \to [0, \infty]$ (defined on *all* subsets) with $\mu^*(\emptyset) = 0$; monotonicity $A \subset B \Rightarrow \mu^*(A) \le \mu^*(B)$; countable *sub*additivity $\mu^*\big(\bigcup_i A_i\big) \le \sum_i \mu^*(A_i)$
- **Carathéodory criterion**: $A$ is $\mu^*$-measurable iff
$$\mu^*(B) = \mu^*(B \cap A) + \mu^*(B \cap A^c) \quad \text{for every } B \subset \Omega$$
  — $A$ slices every test set additively
- **Complete measure**: every subset of a null set is measurable (and null)
- **Lebesgue outer measure**: $\lambda^*(A) = \inf\big\{\sum_i \operatorname{vol}(C_i) : C_i$ open cuboids, $A \subset \bigcup_i C_i\big\}$. The **Lebesgue measure** $\lambda$ is its restriction to the σ-algebra $\mathcal{L}$ of $\lambda^*$-measurable sets
- **Regular** (Borel measure): $\mu(A) = \sup\{\mu(K) : K \subset A$ compact$\}$ (inner) and $\mu(A) = \inf\{\mu(O) : O \supset A$ open$\}$ (outer)
- **Hausdorff measure**: with $d(U)$ = diameter,
$$\mathcal{H}_s(A) = \lim_{\delta \to 0}\, \inf\Big\{\sum_i d(U_i)^s : A \subset \bigcup_i U_i,\ d(U_i) \le \delta\Big\}$$
- **Hausdorff dimension**: $\dim_{\mathcal{H}}(A) = \inf\{s \ge 0 : \mathcal{H}_s(A) = 0\}$ — the critical exponent where $\mathcal{H}_s(A)$ drops from $\infty$ to $0$
- **Measurable function** $f: \Omega \to \bar{\mathbb{R}}$: $f^{-1}(B) \in \mathcal{F}$ for every Borel $B \subset \bar{\mathbb{R}}$. Equivalent test: $\{f > \alpha\} \in \mathcal{F}$ for every $\alpha \in \mathbb{R}$
- **Simple function**: measurable with finite range; canonical form $f = \sum_{i=1}^n \lambda_i \mathbf{1}_{A_i}$ with distinct $\lambda_i$ and disjoint $A_i \in \mathcal{F}$. Its integral: $\sum_i \lambda_i \mu(A_i)$
- **Integrable**: $f$ measurable with $\int_\Omega |f| \, d\mu < \infty$; then $\int f \, d\mu = \int f^+ d\mu - \int f^- d\mu$ where $f^\pm = \max(\pm f, 0)$
- **$L^p(\Omega)$**, $1 \le p < \infty$: measurable functions with $\int |f|^p \, d\mu < \infty$, *modulo* $f \sim g \Leftrightarrow f = g$ a.e.; norm $\|f\|_p = \big(\int_\Omega |f|^p \, d\mu\big)^{1/p}$
- **$L^\infty(\Omega)$**: essentially bounded functions mod $\sim$, with $\|f\|_\infty = \operatorname{ess\,sup} |f| = \inf\{c \in \mathbb{R} : \mu(\{|f| \ge c\}) = 0\}$
- **Uniformly convex**: for every $\varepsilon > 0$ there is $\delta > 0$ such that $\|f\| = \|g\| = 1$ and $\|f - g\| \ge \varepsilon$ imply $\big\|\tfrac{f+g}{2}\big\| \le 1 - \delta$

## Ch. 4 — [[Dual Spaces and Weak Topologies]]

- **Locally convex space (LCS)**: a TVS whose topology is induced by a family of seminorms $(p_i)_{i \in I}$ — the sets $\{y : p_i(x - y) < \varepsilon\}$ form a neighborhood subbase at each $x$ — where the family **separates points**: $x \neq 0 \Rightarrow p_i(x) \neq 0$ for some $i$ (this forces Hausdorff)
- **Weak topology** $\sigma(X, X^*)$ on $X$: the LCS topology of the seminorms $x \mapsto |f(x)|$, $f \in X^*$ — equivalently, the initial topology induced by $X^*$
- **Weak convergence**: $x_n \rightharpoonup x \iff f(x_n) \to f(x)$ for every $f \in X^*$
- **Weak-\* topology** $\sigma(X^*, X)$ on $X^*$: the LCS topology of the seminorms $f \mapsto |f(x)|$, $x \in X$ — the initial topology of the evaluation maps
- **Weak-\* convergence**: $f_n \stackrel{*}{\rightharpoonup} f \iff f_n(x) \to f(x)$ for every $x \in X$ (pointwise on $X$)
- **Schur property**: weakly convergent sequences converge in norm — $x_n \rightharpoonup x \Rightarrow \|x_n - x\| \to 0$. Holds in $\ell^1$

## Ch. 5 — [[Hilbert Spaces]]

- **Inner product** on $X$: $\langle \cdot, \cdot \rangle : X \times X \to \mathbb{K}$ with (i) $\langle x, x \rangle \ge 0$, and $\langle x, x \rangle = 0 \Leftrightarrow x = 0$; (ii) linearity in the **second** slot (course convention): $\langle x, \lambda y + \mu z \rangle = \lambda \langle x, y \rangle + \mu \langle x, z \rangle$; (iii) conjugate symmetry: $\langle x, y \rangle = \overline{\langle y, x \rangle}$
- **Hilbert space**: an inner product space complete w.r.t. $\|v\| = \langle v, v \rangle^{1/2}$
- **Parallelogram law**: $\|u + v\|^2 + \|u - v\|^2 = 2\|u\|^2 + 2\|v\|^2$. A norm comes from an inner product iff it satisfies this
- **Polarization**: recovers the inner product from the norm — real case $\langle u, v \rangle = \tfrac14\big(\|u+v\|^2 - \|u-v\|^2\big)$; the complex case adds the terms $\mp \tfrac{i}{4}\|u \pm iv\|^2$
- **Orthogonal**: $x \perp y \iff \langle x, y \rangle = 0$; then Pythagoras $\|x + y\|^2 = \|x\|^2 + \|y\|^2$
- **Orthogonal complement**: $U^\perp = \{x \in H : \langle x, u \rangle = 0 \ \forall u \in U\}$ — always a closed subspace, and $U^\perp = (\overline{\operatorname{span}} U)^\perp$
- **Orthonormal set**: pairwise orthogonal vectors of norm 1
- **Orthonormal basis (ONB)**: a *maximal* orthonormal set. Equivalent: no nonzero vector is orthogonal to all of it; equivalently its span is dense
- **Orthogonal projection** $P_V$ ($V$ a closed subspace): $P_V x$ = the unique $y \in V$ with $\|x - y\| = \operatorname{dist}(x, V)$ — equivalently the unique $y \in V$ with $x - y \in V^\perp$
- **Unconditional convergence** of $\sum_{i \in I} x_i$: the support $\{i : x_i \neq 0\}$ is countable and *every* enumeration of it gives a series converging to the same limit
- **Dual operator**: for $T \in BL(X, Y)$, $T': Y^* \to X^*$, $f \mapsto f \circ T$; always $\|T'\| = \|T\|$
- **Hilbert adjoint** $T^*$: the unique operator with $\langle x, Ty \rangle = \langle T^* x, y \rangle$ for all $x, y \in H$; satisfies $\|T^*\| = \|T\|$ and $\|T^* T\| = \|T\|^2$
- **Self-adjoint**: $T^* = T$. **Unitary**: $T^* T = T T^* = I$. **Normal**: $T^* T = T T^*$
- **Hilbert space isomorphism (unitary map)** $T: H_1 \to H_2$: bijective with $\langle Tx, Ty \rangle = \langle x, y \rangle$ for all $x, y$

## Ch. 6 — [[Spectral Theory]]

($X$ a complex Banach space, $A \in BL(X)$; write $A - \lambda$ for $A - \lambda I$.)

- **Resolvent set**: $\rho(A) = \{\lambda \in \mathbb{C} : (A - \lambda)^{-1}$ exists in $BL(X)\}$. The **resolvent operator** at $\lambda \in \rho(A)$: $R_\lambda(A) = (A - \lambda)^{-1}$
- **Spectrum**: $\sigma(A) = \mathbb{C} \setminus \rho(A)$ — always compact and non-empty
- **Point spectrum** $\sigma_p$: $\{\lambda : A - \lambda$ not injective$\}$. These $\lambda$ are the **eigenvalues**; any $0 \neq x \in \ker(A - \lambda)$ is an **eigenvector**
- **Continuous spectrum** $\sigma_c$: $\{\lambda : A - \lambda$ injective, not surjective, with dense range$\}$
- **Residual spectrum** $\sigma_r$: $\{\lambda : A - \lambda$ injective, with non-dense range$\}$. Together: $\sigma = \sigma_p \sqcup \sigma_c \sqcup \sigma_r$
- **Spectral radius**: $r(A) = \inf_n \|A^n\|^{1/n} = \lim_{n \to \infty} \|A^n\|^{1/n}$; equals $\max\{|\lambda| : \lambda \in \sigma(A)\}$
- **Numerical range** (Hilbert case): $W(T) = \{\langle x, Tx \rangle : \|x\| = 1\}$; always $\sigma(T) \subset \overline{W(T)}$
- **Compact operator**: $K \in BL(X, Y)$ such that $\overline{K(B)}$ is compact for every bounded $B \subset X$ — equivalently, every bounded sequence $(x_n)$ admits a norm-convergent subsequence of $(K x_n)$
- **Finite rank**: $\dim \operatorname{range}(T) < \infty$. Finite rank ⇒ compact
- **Invariant subspace** of $T$: a closed subspace $M$ with $T(M) \subset M$
- **Fredholm operator**: $S$ with $\dim \ker(S) < \infty$, $\operatorname{range}(S)$ closed, and $\operatorname{codim}(\operatorname{range}(S)) = \dim\big(X / \operatorname{range}(S)\big) < \infty$
- **Fredholm index**: $\operatorname{ind}(S) = \dim \ker(S) - \operatorname{codim}(\operatorname{range}(S))$. For $T$ compact, $S = T - I$ is Fredholm of index $0$ — so injectivity ⇔ surjectivity

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]] · source: [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]].
