---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [Hilbert space, inner product space, orthonormal basis, Parseval, Riesz representation, adjoint operator]
---

# Hilbert Spaces

> **Where this sits.** Chapter 5 of [[18.102 Functional Analysis]] — the Banach space with *geometry*. An inner product adds angles and orthogonality to the norm, and every pathology of general [[Normed and Banach Spaces|Banach spaces]] softens: closest points exist, the dual is the space itself, bases behave like $\mathbb{R}^n$'s. The model: $L^2$ — the reason quantum mechanics and PDE theory live here. Every entry: *Idea* → *Formally* → *Mechanism*.

## The structure

**Inner product, Hilbert space.** *Idea:* an abstract dot product — a pairing that measures both length ($\langle x,x \rangle$) and alignment ($\langle x,y \rangle$); Hilbert means additionally complete in the induced length. *Formally:* $\langle x,x \rangle \ge 0$ with $= 0 \Leftrightarrow x = 0$; $\langle x, \lambda y + \mu z \rangle = \lambda \langle x,y \rangle + \mu \langle x,z \rangle$ (linear in the **second** slot — course convention); $\langle x,y \rangle = \overline{\langle y,x \rangle}$; complete w.r.t. $\|v\| = \langle v,v \rangle^{1/2}$. Examples: $\ell^2$; $L^2(\Omega)$ with $\langle f,g \rangle = \int \bar{f} g \, d\mu$.

**Cauchy–Schwarz.** *Idea:* alignment never exceeds the product of lengths, with equality exactly for parallel vectors — the inequality that makes the norm a norm. *Formally:* $|\langle v,w \rangle| \le \|v\| \|w\|$, equality iff linearly dependent. *Mechanism:* minimize the quadratic $t \mapsto \|v + t w\|^2 \ge 0$ over $t$.

**Parallelogram law and polarization.** *Idea:* inner-product norms have a fingerprint — the parallelogram identity — and it is a *test*: a norm comes from an inner product **iff** it passes, in which case polarization reconstructs the product from lengths alone. *Formally:*
$$\|u+v\|^2 + \|u-v\|^2 = 2\|u\|^2 + 2\|v\|^2, \qquad \langle u,v \rangle = \tfrac14\big(\|u+v\|^2 - \|u-v\|^2\big) \ (\text{real; complex adds } \mp\tfrac{i}{4}\|u \pm iv\|^2)$$
$\ell^p, L^p$ pass iff $p = 2$ — $L^2$ is the *only* Hilbert space in the family.

## Projection — the geometric miracle

**Closest-point / projection theorem.** *Idea:* against any closed subspace, every point has a unique shadow — the nearest point — and the error is exactly perpendicular. This single fact is what most of Hilbert space theory runs on. *Formally:* $V \neq \{0\}$ closed subspace, $x \in H$:
$$\exists!\, y \in V: \ \|x - y\| = \operatorname{dist}(x, V), \qquad \text{characterized by } x - y \in V^\perp$$
*Mechanism:* for a minimizing sequence, the parallelogram law gives $\|y_n - y_m\|^2 \le 2\|y_n - x\|^2 + 2\|y_m - x\|^2 - 4\operatorname{dist}(x,V)^2 \to 0$ — Cauchy; completeness finishes. Geometry + completeness, clicking together.

**Consequences.** *Idea:* the whole space splits orthogonally along any closed subspace — in Hilbert spaces every closed subspace has a canonical complement (generic Banach spaces refuse: $c_0 \subset \ell^\infty$ has none). *Formally:* $P_V: x \mapsto y$ is linear, $P_V^2 = P_V$, $\|P_V\| = 1$, $\ker P_V = V^\perp$, and
$$H = V \oplus V^\perp \ \text{ for every closed subspace } V, \qquad \overline{V} = (V^\perp)^\perp$$
Extends to closed *convex* sets and to uniformly convex Banach spaces — the form used in optimization.

## Riesz representation — the dual is the space

**Riesz representation theorem.** *Idea:* every continuous linear measurement on a Hilbert space is "take the inner product with one fixed vector" — the dual space is a mirror image of the space itself. *Formally:*
$$\boxed{\;\forall f \in H^*\ \exists!\, z_f \in H: \ f(x) = \langle z_f, x \rangle \ \forall x, \qquad \|f\| = \|z_f\|\;}$$
and $f \mapsto z_f$ is an anti-linear isometric bijection $H^* \cong H$ (quantum mechanics' bra–ket: $|\psi\rangle \leftrightarrow \langle\psi|$). *Mechanism:* $\ker(f)$ is a closed hyperplane ⇒ $\ker(f)^\perp$ is one-dimensional (projection theorem); scale its generator.

**Payoffs.** *Formally:* $H$ reflexive; weak convergence is internal — $x_n \rightharpoonup x \Leftrightarrow \langle y, x_n - x \rangle \to 0\ \forall y \in H$; and **every bounded sequence in $H$ has a weakly convergent subsequence** (via [[Dual Spaces and Weak Topologies|Banach–Alaoglu]] on a separable closed span) — the cleanest compactness statement in the course.

## Orthonormal bases — Fourier analysis abstracted

**ONB.** *Idea:* the right notion of basis here is not "spans by finite sums" but "orthonormal and impossible to enlarge" — equivalently, dense span, equivalently nothing is orthogonal to all of it. *Formally:* orthonormal $S$ ($\|e\| = 1$, distinct elements orthogonal) with
$$S \text{ maximal} \iff \big(\langle x, e \rangle = 0\ \forall e \in S \Rightarrow x = 0\big) \iff \overline{\operatorname{span}}(S) = H$$

**Bessel and Parseval.** *Idea:* against any orthonormal family, a vector's coefficients can't carry more energy than the vector (Bessel); against a *basis*, they carry exactly all of it, and the vector is the sum of its projections in any order (Parseval) — Pythagoras, infinitely iterated; for trigonometric bases this *is* Fourier series. *Formally:*
$$\text{Bessel: } \sum_{i \in I} |\langle e_i, x \rangle|^2 \le \|x\|^2 \qquad\quad \text{Parseval (ONB): } x = \sum_{i \in I} \langle e_i, x \rangle e_i \ \text{(unconditional)}, \quad \|x\|^2 = \sum_{i \in I} |\langle e_i, x \rangle|^2$$
*Mechanism (Bessel):* split $x = u + v$, $u$ = partial projection, $u \perp v$, apply Pythagoras; only countably many coefficients are nonzero even for uncountable $I$. *Mechanism (Parseval):* Bessel makes the partial sums Cauchy; the limit has the same coefficients as $x$, so maximality forces equality.

**Existence and classification.** *Idea:* every Hilbert space has an ONB, and in the separable infinite-dimensional case the coefficient map is an isomorphism onto $\ell^2$ — *"there is essentially only one Hilbert space."* The interesting structure is never the space; it's the operators on it. *Formally:* ONB exists (Zorn on orthonormal sets); $H$ separable ⇔ every ONB countable; $\dim H = \infty$ separable ⇒ $H \cong \ell^2$ via $x \mapsto (\langle e_i, x \rangle)_i$. Fine print: unconditional ≠ absolute convergence in infinite dimensions (Dvoretzky–Rogers).

## Adjoints — the $*$ operation

**Hilbert adjoint.** *Idea:* every bounded operator has a mirror partner: whatever $T$ does inside the right slot of the inner product, $T^*$ does inside the left — the abstract conjugate transpose, obtained by pulling the [[Normed and Banach Spaces|dual operator]] $T': f \mapsto f \circ T$ back through Riesz. *Formally:*
$$T^* \in BL(H) \text{ unique with } \langle x, Ty \rangle = \langle T^* x, y \rangle \ \forall x,y, \qquad \|T^*\| = \|T\|$$

**The calculus.** *Formally:* $(ST)^* = T^* S^*$, $T^{**} = T$, $(\lambda T)^* = \bar{\lambda} T^*$; the **C\*-identity** $\|T^* T\| = \|T\|^2$ (seed of C*-algebra theory); and the structural duality — abstract rank–nullity, used verbatim in [[Spectral Theory|Fredholm theory]]:
$$\ker S = (\operatorname{range} S^*)^\perp, \qquad \ker S^* = (\operatorname{range} S)^\perp$$

**The zoo.** *Formally:* **self-adjoint** $T^* = T$; **unitary** $T^* T = T T^* = I$ (the isomorphisms); **normal** $T^* T = T T^*$. Matrices: $*$ = conjugate transpose. Shift on $\ell^2$: $S(x_1, x_2, \dots) = (x_2, x_3, \dots)$, $S^*(x_1, x_2, \dots) = (0, x_1, x_2, \dots)$; $S S^* = I$ but $S^* S = P_{\{x_1 = 0\}}$ — not normal.

**Hellinger–Toeplitz.** *Idea:* symmetry on the whole space secretly implies continuity — so genuinely unbounded self-adjoint operators (position, momentum, every Hamiltonian) *cannot* be everywhere-defined; their restricted domains are forced by mathematics, not pedantry. *Formally:* $\langle Tx, y \rangle = \langle x, Ty \rangle\ \forall x, y$ with $\operatorname{dom}(T) = H$ ⇒ $T \in BL(H)$. *Mechanism:* symmetry closes the graph; closed graph theorem.

**The norm on the diagonal.** *Idea:* for self-adjoint operators the norm is visible in the quadratic form alone — the lever that makes the [[Spectral Theory|spectral theorem]] work. *Formally:* $T = T^*$ ⇒ $\|T\| = \sup_{\|x\| = 1} |\langle x, Tx \rangle|$.

## Related

- [[Normed and Banach Spaces]] — the general theory this specializes
- [[Dual Spaces and Weak Topologies]] — weak compactness, at its best here
- [[Measure Theory and the Lebesgue Integral]] — $L^2$, the main example
- [[Spectral Theory]] — what adjoints are for
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 5

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
