---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [Hilbert space, inner product space, orthonormal basis, Parseval, Riesz representation, adjoint operator]
---

# Hilbert Spaces

> **Where this sits.** Chapter 5 of [[18.102 Functional Analysis]] — the Banach space with *geometry*. An inner product adds angles and orthogonality to the norm, and every pathology of general [[Normed and Banach Spaces|Banach spaces]] softens: closest points exist, the dual is the space itself, bases behave like $\mathbb{R}^n$'s. The model: $L^2$ — the reason quantum mechanics and PDE theory live here.

## The structure

- **Inner product**: positive definite, conjugate-symmetric, linear in the **second** slot (this course's convention — many books use the first). **Hilbert space** = complete inner product space. Examples: $\ell^2$, $L^2(\Omega)$ with $\langle f,g\rangle = \int \bar{f} g \, d\mu$
- **Cauchy–Schwarz** $|\langle v,w\rangle| \le \|v\| \|w\|$ (minimize the quadratic $\|v + tw\|^2$); makes $\|v\| = \langle v,v\rangle^{1/2}$ a norm and the inner product continuous
- **Parallelogram law** $\|u{+}v\|^2 + \|u{-}v\|^2 = 2\|u\|^2 + 2\|v\|^2$ — the *fingerprint*: a norm comes from an inner product **iff** it satisfies this (recover it via polarization). $\ell^p, L^p$ pass iff $p = 2$ — so $L^2$ is the *only* Hilbert space in the family

## Projection — the geometric miracle

**Closest-point theorem.** $V$ closed subspace, $x \in H$: there is a **unique** closest point $P_V x \in V$, characterized by $x - P_V x \perp V$. *Mechanism:* the parallelogram law forces any minimizing sequence to be Cauchy — this is where completeness + geometry click together. Consequences:

- $H = V \oplus V^\perp$ for every closed subspace — **every closed subspace is complemented** (fails in Banach spaces: $c_0 \subset \ell^\infty$ has no complement); $P_V$ is a norm-1 linear projection; $\overline{V} = (V^\perp)^\perp$
- Extends to closed *convex* sets (and to uniformly convex Banach spaces) — the form used in optimization

## Riesz representation — the dual is the space

$$\boxed{\;\text{every } f \in H^* \text{ is } f = \langle z_f, \cdot \rangle \text{ for a unique } z_f \in H, \quad \|f\| = \|z_f\|\;}$$

*Mechanism:* $\ker(f)$ is a closed hyperplane; its orthogonal complement is one-dimensional; scale. The map $f \mapsto z_f$ is an **anti-linear isometric bijection** $H^* \cong H$ (quantum mechanics' bra–ket: $|\psi\rangle \leftrightarrow \langle\psi|$). Immediate payoffs: $H$ is **reflexive**; weak convergence is testable inside $H$ ($x_n \rightharpoonup x$ ⇔ $\langle y, x_n - x\rangle \to 0\ \forall y$); **every bounded sequence has a weakly convergent subsequence** (via [[Dual Spaces and Weak Topologies|Banach–Alaoglu]]) — the cleanest compactness statement in the course.

## Orthonormal bases — Fourier analysis abstracted

**ONB** = *maximal* orthonormal set. Three equivalent tests: maximal ⇔ only $0$ is orthogonal to all of it ⇔ its span is dense.

- **Bessel** (any orthonormal family): $\sum_i |\langle e_i, x\rangle|^2 \le \|x\|^2$ — coefficients are always square-summable, and only countably many are nonzero even for uncountable families
- **Parseval** (ONB, the equality): $x = \sum_i \langle e_i, x\rangle e_i$ (unconditional convergence — any order works) and $\|x\|^2 = \sum_i |\langle e_i, x\rangle|^2$. Generalized Pythagoras; for trigonometric ONBs this *is* the theory of Fourier series
- Every Hilbert space has an ONB (Zorn); separable ⇔ ONB countable. **Classification**: every infinite-dimensional separable Hilbert space is isomorphic to $\ell^2$ via $x \mapsto (\langle e_i, x\rangle)_i$ — *"there is essentially only one Hilbert space."* The interesting structure is never the space; it's the operators on it
- Fine print worth keeping: unconditional ≠ absolute convergence in infinite dimensions (Dvoretzky–Rogers: every infinite-dimensional Banach space has unconditionally-but-not-absolutely convergent series)

## Adjoints — the $*$ operation

Via Riesz, the [[Normed and Banach Spaces|dual operator]] pulls back to the space itself: $T^* \in BL(H)$ is defined by $\langle x, Ty\rangle = \langle T^* x, y\rangle$, with $\|T^*\| = \|T\|$.

- Calculus: $(ST)^* = T^* S^*$, $T^{**} = T$, and the **C\*-identity** $\|T^* T\| = \|T\|^2$ (the seed of C\*-algebra theory). Structural duality: $\ker S = (\operatorname{range} S^*)^\perp$ — the abstract rank–nullity, used verbatim in [[Spectral Theory|Fredholm theory]]
- Zoo: **self-adjoint** ($T^* = T$), **unitary** ($T^*T = TT^* = I$, the isomorphisms), **normal** ($T^*T = TT^*$). Matrix case: $*$ = conjugate transpose. The shift on $\ell^2$: adjoint is the *other* shift; not normal — one composition is $I$, the other a projection
- **Hellinger–Toeplitz**: a symmetric operator defined on *all* of $H$ is automatically bounded (closed graph theorem). Deep consequence: unbounded self-adjoint operators — position, momentum, every Hamiltonian — *cannot* be everywhere-defined; domains are forced by the mathematics, not pedantry
- For self-adjoint $T$: $\|T\| = \sup_{\|x\|=1} |\langle x, Tx\rangle|$ — the norm is visible on the diagonal; the lever that makes the [[Spectral Theory|spectral theorem]] work

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
