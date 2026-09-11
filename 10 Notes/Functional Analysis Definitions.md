---
type: reference
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [FA definitions, functional analysis glossary]
---

# Functional Analysis Definitions

> The load-bearing definitions of [[18.102 Functional Analysis]], stated precisely, grouped by chapter. Convention throughout: $\mathbb{K} \in \{\mathbb{R}, \mathbb{C}\}$. For theorems and context, follow the chapter links.

## Ch. 1 — [[Topological and Metric Spaces]]

- **Partial order** on $P$: relation $\le$ with $a \le a$; $a \le b \wedge b \le c \Rightarrow a \le c$; $a \le b \wedge b \le a \Rightarrow a = b$. **Chain**: $C \subset P$ totally ordered. **Upper bound** of $A$: $x$ with $a \le x\ \forall a \in A$. **Maximal**: $x \le y \Rightarrow x = y$. **Zorn (axiom)**: a non-empty poset in which every non-empty chain has an upper bound has a maximal element
- **Hamel basis**: $B \subset V$ linearly independent (every finite subfamily) with $\operatorname{span}(B) = V$ (finite linear combinations)
- **Topology** on $X$: $\mathcal{T} \subset \mathcal{P}(X)$ with (i) $\emptyset, X \in \mathcal{T}$; (ii) $U_i \in \mathcal{T},\ i \in I$ ⇒ $\bigcup_{i \in I} U_i \in \mathcal{T}$; (iii) $U_1, \dots, U_n \in \mathcal{T}$ ⇒ $\bigcap_{i=1}^n U_i \in \mathcal{T}$. $A$ **closed** iff $A^c \in \mathcal{T}$
- **Neighborhood** of $x$: $A \subset X$ such that $\exists U \in \mathcal{T}$ with $x \in U \subset A$
- **Hausdorff**: $\forall x \neq y\ \exists$ neighborhoods $U_x \ni x$, $U_y \ni y$ with $U_x \cap U_y = \emptyset$
- **Interior** $\operatorname{int}(A)$: the largest open set $\subset A$ (= points having a neighborhood inside $A$). **Closure** $\overline{A}$: the smallest closed set $\supset A$. **Boundary** $\partial A$: $\{x : $ every neighborhood of $x$ meets both $A$ and $A^c\}$. **Dense**: $\overline{A} = X$
- **Convergence**: $x_n \to x$ iff for every neighborhood $U$ of $x$, $\exists N: x_n \in U\ \forall n \ge N$
- **Continuous** $f: X \to Y$: $f^{-1}(U) \in \mathcal{T}_X$ for every $U \in \mathcal{T}_Y$. **Sequentially continuous**: $x_n \to x \Rightarrow f(x_n) \to f(x)$. (Equivalent when $X$ is first countable)
- **Initial topology** induced by $(f_i: X \to X_i)_{i \in I}$: the coarsest topology on $X$ making every $f_i$ continuous. **Product topology** on $\prod_i X_i$: initial topology of the projections $\pi_j$
- **Base** $\mathcal{B} \subset \mathcal{T}$: every $U \in \mathcal{T}$ is a union of elements of $\mathcal{B}$. **Subbase**: finite intersections form a base. **First countable**: every point has a countable neighborhood base. **Second countable**: countable base. **Separable**: $\exists$ countable $D$ with $\overline{D} = X$
- **Compact** $K$: every open cover of $K$ admits a finite subcover. **Sequentially compact**: every sequence in $K$ has a subsequence converging in $K$. **Relatively compact**: $\overline{K}$ compact. (Compact ⇔ sequentially compact in metric spaces)
- **Topological vector space**: vector space $V$ with a Hausdorff topology making $+ : V \times V \to V$ and $\cdot : \mathbb{K} \times V \to V$ continuous
- **Metric** on $X$: $d: X \times X \to [0,\infty)$ with $d(x,y) = 0 \Leftrightarrow x = y$; $d(x,y) = d(y,x)$; $d(x,y) \le d(x,z) + d(z,y)$. Ball $B_\varepsilon(x) = \{y : d(x,y) < \varepsilon\}$
- **Cauchy**: $\forall \varepsilon > 0\ \exists N: d(x_n, x_m) < \varepsilon\ \forall n,m \ge N$. **Complete**: every Cauchy sequence converges in $X$
- **Isometry**: $\Phi: X \to Y$ with $d_Y(\Phi x, \Phi y) = d_X(x,y)\ \forall x,y$. **Completion** of $(X,d)$: complete $(X', d')$ with an isometry $\Phi: X \to X'$ such that $\Phi(X)$ is dense in $X'$ (unique up to isometry)
- **Hölder conjugates**: $p, q \in [1, \infty]$ with $\tfrac1p + \tfrac1q = 1$
- **Sequence spaces** over $\mathbb{K}$: $\ell^p = \{(x_n) : \sum_n |x_n|^p < \infty\}$ with $\|x\|_p = (\sum_n |x_n|^p)^{1/p}$, $1 \le p < \infty$; $\ell^\infty = \{(x_n) : \sup_n |x_n| < \infty\}$ with $\|x\|_\infty = \sup_n |x_n|$; $c_0 = \{(x_n) \in \ell^\infty : \lim_n x_n = 0\}$; $c_c = \{(x_n) : x_n = 0$ for all but finitely many $n\}$
- **Nowhere dense**: $\operatorname{int}(\overline{A}) = \emptyset$. **Meager** (1st category): countable union of nowhere dense sets. **Residual / Baire-generic**: complement of a meager set

## Ch. 2 — [[Normed and Banach Spaces]]

- **Norm** on $V$: $\|\cdot\|: V \to [0,\infty)$ with $\|\alpha v\| = |\alpha| \|v\|$; $\|v + w\| \le \|v\| + \|w\|$; $\|v\| = 0 \Rightarrow v = 0$. **Seminorm**: without the last property. Induced metric: $d(v,w) = \|v - w\|$
- **Banach space**: normed space complete w.r.t. the induced metric
- **Schauder basis** $\{e_n\}_{n \in \mathbb{N}}$: for every $x \in X$ there is a *unique* $(x_n) \subset \mathbb{K}$ with $\lim_{N \to \infty} \|x - \sum_{n=1}^N x_n e_n\| = 0$
- **Bounded operator**: linear $T: \operatorname{dom}(T) \subset V \to W$ with $\|T\| = \sup_{0 \neq x \in \operatorname{dom}(T)} \frac{\|Tx\|_W}{\|x\|_V} < \infty$ (⇔ $T$ continuous). $BL(V, W)$ = bounded linear maps $V \to W$, normed by $\|\cdot\|_{V \to W}$
- **Dual space**: $X^* = BL(X, \mathbb{K})$ (always Banach). **Algebraic dual** $X'$: all linear $f: X \to \mathbb{K}$
- **Sublinear functional**: $p: V \to \mathbb{R}$ with $p(x+y) \le p(x) + p(y)$ and $p(\lambda x) = \lambda p(x)\ \forall \lambda \ge 0$
- **Bidual**: $X^{**} = (X^*)^*$. **Canonical embedding**: $i: X \to X^{**}$, $i(x)(f) = f(x)$ — always a linear isometry. **Reflexive**: $i$ surjective
- **Closed operator**: $T: \operatorname{dom}(T) \subset V \to W$ linear with $\operatorname{graph}(T) = \{(x, Tx) : x \in \operatorname{dom}(T)\}$ closed in $V \times W$ (norm $\|(v,w)\| = \|v\| + \|w\|$). Sequentially: $x_n \to x$, $Tx_n \to y$ $\Rightarrow$ $x \in \operatorname{dom}(T)$ and $y = Tx$

## Ch. 3 — [[Measure Theory and the Lebesgue Integral]]

- **σ-algebra** $\mathcal{F} \subset \mathcal{P}(\Omega)$: $\emptyset \in \mathcal{F}$; $A \in \mathcal{F} \Rightarrow A^c \in \mathcal{F}$; $A_i \in \mathcal{F},\ i \in \mathbb{N}$ ⇒ $\bigcup_i A_i \in \mathcal{F}$. Generated σ-algebra $\sigma(\mathcal{A})$: the smallest σ-algebra containing $\mathcal{A}$
- **Measure**: $\mu: \mathcal{F} \to [0, \infty]$ with $\mu(\emptyset) = 0$ and $\mu(\bigcup_i A_i) = \sum_i \mu(A_i)$ for pairwise disjoint $A_i \in \mathcal{F}$. **Null set**: $\mu(A) = 0$. **Almost everywhere**: off a null set. **Finite / σ-finite**: $\mu(\Omega) < \infty$ / $\Omega = \bigcup_n A_n$ with $\mu(A_n) < \infty$
- **Borel σ-algebra** $\mathcal{B}$: generated by the topology; **Borel measure**: measure on $\mathcal{B}$ finite on compacts
- **Outer measure**: $\mu^*: \mathcal{P}(\Omega) \to [0,\infty]$ with $\mu^*(\emptyset) = 0$; $A \subset B \Rightarrow \mu^*(A) \le \mu^*(B)$; $\mu^*(\bigcup_i A_i) \le \sum_i \mu^*(A_i)$
- **Carathéodory criterion**: $A$ is $\mu^*$-measurable iff $\mu^*(B) = \mu^*(B \cap A) + \mu^*(B \cap A^c)$ for **every** $B \subset \Omega$
- **Complete measure**: $A \subset N \in \mathcal{F}$, $\mu(N) = 0$ ⇒ $A \in \mathcal{F}$ (and $\mu(A) = 0$)
- **Lebesgue outer measure**: $\lambda^*(A) = \inf\{\sum_i \operatorname{vol}(C_i) : C_i$ open cuboids, $A \subset \bigcup_i C_i\}$; **Lebesgue measure** $\lambda$: its restriction to the σ-algebra $\mathcal{L}$ of $\lambda^*$-measurable sets
- **Regular** Borel measure: $\mu(A) = \sup\{\mu(K) : K \subset A$ compact$\}$ (inner) and $\mu(A) = \inf\{\mu(O) : O \supset A$ open$\}$ (outer)
- **Hausdorff measure**: $\mathcal{H}_s(A) = \lim_{\delta \to 0} \inf\{\sum_i d(U_i)^s : A \subset \bigcup_i U_i,\ d(U_i) \le \delta\}$ ($d$ = diameter). **Hausdorff dimension**: $\dim_{\mathcal{H}}(A) = \inf\{s \ge 0 : \mathcal{H}_s(A) = 0\}$
- **Measurable function** $f: \Omega \to \bar{\mathbb{R}}$: $f^{-1}(B) \in \mathcal{F}$ for every Borel $B \subset \bar{\mathbb{R}}$ (equivalently: $\{f > \alpha\} \in \mathcal{F}\ \forall \alpha \in \mathbb{R}$)
- **Simple function**: measurable with finite range; canonical form $\sum_{i=1}^n \lambda_i \mathbf{1}_{A_i}$, $\lambda_i$ distinct, $A_i \in \mathcal{F}$ disjoint. Its integral: $\int = \sum_i \lambda_i \mu(A_i)$
- **Integrable**: $f$ measurable with $\int_\Omega |f| \, d\mu < \infty$; then $\int f \, d\mu = \int f^+ d\mu - \int f^- d\mu$, $f^\pm = \max(\pm f, 0)$
- **$L^p(\Omega)$**, $1 \le p < \infty$: $\{f$ measurable$: \int |f|^p d\mu < \infty\} / \sim$ where $f \sim g$ iff $f = g$ a.e.; $\|f\|_p = (\int_\Omega |f|^p \, d\mu)^{1/p}$. **$L^\infty(\Omega)$**: essentially bounded functions mod $\sim$; $\|f\|_\infty = \operatorname{ess\,sup}|f| = \inf\{c : \mu(\{|f| \ge c\}) = 0\}$
- **Uniformly convex**: $\forall \varepsilon > 0\ \exists \delta > 0$: $\|f\| = \|g\| = 1,\ \|f - g\| \ge \varepsilon \Rightarrow \|\tfrac{f+g}{2}\| \le 1 - \delta$

## Ch. 4 — [[Dual Spaces and Weak Topologies]]

- **Locally convex space**: TVS whose topology is induced by a family of seminorms $(p_i)_{i \in I}$ — the sets $\{y : p_i(x - y) < \varepsilon\}$ form a neighborhood subbase at $x$ — which **separates points**: $x \neq 0 \Rightarrow \exists i: p_i(x) \neq 0$
- **Weak topology** $\sigma(X, X^*)$ on $X$: the LCS topology of the seminorms $x \mapsto |f(x)|$, $f \in X^*$; equivalently the initial topology induced by $X^*$. Convergence: $x_n \rightharpoonup x \Leftrightarrow f(x_n) \to f(x)\ \forall f \in X^*$
- **Weak-\* topology** $\sigma(X^*, X)$ on $X^*$: the LCS topology of the seminorms $f \mapsto |f(x)|$, $x \in X$; the initial topology of the evaluations. Convergence: $f_n \stackrel{*}{\rightharpoonup} f \Leftrightarrow f_n(x) \to f(x)\ \forall x \in X$
- **Schur property**: $x_n \rightharpoonup x \Rightarrow \|x_n - x\| \to 0$ (holds in $\ell^1$)

## Ch. 5 — [[Hilbert Spaces]]

- **Inner product** on $X$: $\langle \cdot, \cdot \rangle : X \times X \to \mathbb{K}$ with (i) $\langle x, x \rangle \ge 0$ and $\langle x, x \rangle = 0 \Leftrightarrow x = 0$; (ii) $\langle x, \lambda y + \mu z \rangle = \lambda \langle x, y \rangle + \mu \langle x, z \rangle$ (linear in the **second** slot — course convention); (iii) $\langle x, y \rangle = \overline{\langle y, x \rangle}$
- **Hilbert space**: inner product space complete w.r.t. $\|v\| = \langle v, v \rangle^{1/2}$
- **Parallelogram law**: $\|u + v\|^2 + \|u - v\|^2 = 2\|u\|^2 + 2\|v\|^2$ — a norm is an inner-product norm iff it satisfies this; the inner product is recovered by **polarization** (real case: $\langle u, v \rangle = \tfrac14(\|u+v\|^2 - \|u-v\|^2)$; complex case adds the $\pm i$ terms)
- **Orthogonal**: $x \perp y \Leftrightarrow \langle x, y \rangle = 0$. **Orthogonal complement**: $U^\perp = \{x \in H : \langle x, u \rangle = 0\ \forall u \in U\}$ (always a closed subspace)
- **Orthonormal set**: pairwise orthogonal, all norms 1. **Orthonormal basis**: a *maximal* orthonormal set (⇔ $\overline{\operatorname{span}}$ dense ⇔ only $0$ is orthogonal to all elements)
- **Orthogonal projection** $P_V$ ($V$ closed subspace): $P_V x$ = the unique $y \in V$ with $\|x - y\| = \operatorname{dist}(x, V)$, equivalently the unique $y \in V$ with $x - y \in V^\perp$
- **Unconditional convergence** of $\sum_{i \in I} x_i$: $\{i : x_i \neq 0\}$ is countable and every enumeration of it yields a series converging to the same limit
- **Dual operator**: $T' : Y^* \to X^*$, $f \mapsto f \circ T$, for $T \in BL(X, Y)$. **Hilbert adjoint** $T^*$: the unique operator with $\langle x, Ty \rangle = \langle T^* x, y \rangle\ \forall x, y \in H$; satisfies $\|T^*\| = \|T\|$
- **Self-adjoint**: $T^* = T$. **Unitary**: $T^* T = T T^* = I$. **Normal**: $T^* T = T T^*$. **Unitary map / Hilbert space isomorphism** $T: H_1 \to H_2$: bijective with $\langle Tx, Ty \rangle = \langle x, y \rangle\ \forall x, y$

## Ch. 6 — [[Spectral Theory]]

($X$ a complex Banach space, $A \in BL(X)$; write $A - \lambda$ for $A - \lambda I$.)

- **Resolvent set**: $\rho(A) = \{\lambda \in \mathbb{C} : (A - \lambda)^{-1}$ exists in $BL(X)\}$. **Resolvent operator**: $R_\lambda(A) = (A - \lambda)^{-1}$ for $\lambda \in \rho(A)$
- **Spectrum**: $\sigma(A) = \mathbb{C} \setminus \rho(A)$, partitioned as: **point spectrum** $\sigma_p$ = $\{\lambda : A - \lambda$ not injective$\}$ (**eigenvalues**; $0 \neq x \in \ker(A - \lambda)$ an **eigenvector**); **continuous spectrum** $\sigma_c$ = $\{\lambda : A - \lambda$ injective, not surjective, $\overline{\operatorname{range}} = X\}$; **residual spectrum** $\sigma_r$ = $\{\lambda : A - \lambda$ injective, $\overline{\operatorname{range}} \neq X\}$
- **Spectral radius**: $r(A) = \inf_n \|A^n\|^{1/n} = \lim_{n \to \infty} \|A^n\|^{1/n}$ (= $\max\{|\lambda| : \lambda \in \sigma(A)\}$)
- **Numerical range**: $W(T) = \{\langle x, Tx \rangle : \|x\| = 1\}$ (Hilbert case); $\sigma(T) \subset \overline{W(T)}$
- **Compact operator**: $K \in BL(X, Y)$ such that $\overline{K(B)}$ is compact for every bounded $B \subset X$ (⇔ every bounded $(x_n)$ has $(K x_{n_k})$ norm-convergent). **Finite rank**: $\dim \operatorname{range}(T) < \infty$
- **Invariant subspace** of $T$: closed subspace $M$ with $T(M) \subset M$
- **Fredholm operator**: $S$ with $\dim \ker(S) < \infty$, $\operatorname{range}(S)$ closed, and $\operatorname{codim}(\operatorname{range}(S)) = \dim(X / \operatorname{range}(S)) < \infty$. **Fredholm index**: $\operatorname{ind}(S) = \dim \ker(S) - \operatorname{codim}(\operatorname{range}(S))$. ($S = T - I$ with $T$ compact: Fredholm of index $0$)

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]] · source: [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]].
