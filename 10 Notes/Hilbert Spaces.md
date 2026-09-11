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

- **Inner product**: $\langle x,x \rangle \ge 0$ with equality iff $x = 0$; $\langle x, \lambda y + \mu z \rangle = \lambda \langle x,y \rangle + \mu \langle x,z \rangle$ (linear in the **second** slot — course convention); $\langle x,y \rangle = \overline{\langle y,x \rangle}$. **Hilbert space** = complete w.r.t. $\|v\| = \langle v,v \rangle^{1/2}$. Examples: $\ell^2$; $L^2(\Omega)$ with $\langle f,g \rangle = \int_\Omega \bar{f} g \, d\mu$
- **Cauchy–Schwarz**: $|\langle v,w \rangle| \le \|v\| \|w\|$, equality iff linearly dependent (minimize the quadratic $t \mapsto \|v + t w\|^2$); makes $\|\cdot\|$ a norm and $\langle \cdot,\cdot \rangle$ continuous
- **Parallelogram law** — the *fingerprint*:
$$\|u+v\|^2 + \|u-v\|^2 = 2\|u\|^2 + 2\|v\|^2$$
  a norm comes from an inner product **iff** it satisfies this; recover it via **polarization** ($\langle u,v \rangle = \tfrac14(\|u+v\|^2 - \|u-v\|^2)$ real; complex adds $\mp i \|u \pm iv\|^2$ terms). $\ell^p, L^p$ pass iff $p = 2$ — $L^2$ is the *only* Hilbert space in the family

## Projection — the geometric miracle

**Closest-point theorem.** $V \neq \{0\}$ closed subspace, $x \in H$:
$$\exists!\; y \in V: \ \|x - y\| = \operatorname{dist}(x, V), \qquad \text{characterized by } x - y \in V^\perp$$
*Mechanism:* for a minimizing sequence, the parallelogram law gives $\|y_n - y_m\|^2 \le 2\|y_n - x\|^2 + 2\|y_m - x\|^2 - 4 \operatorname{dist}(x,V)^2 \to 0$ — Cauchy; completeness finishes. This is where completeness + geometry click together. Consequences:

- $P_V: x \mapsto y$ is linear, $P_V^2 = P_V$, $\|P_V\| = 1$, $\ker P_V = V^\perp$, and
$$H = V \oplus V^\perp \quad \text{for every closed subspace } V, \qquad \overline{V} = (V^\perp)^\perp$$
  — **every closed subspace is complemented** (fails in Banach spaces: $c_0 \subset \ell^\infty$ has no complement)
- Extends to closed *convex* sets, and to uniformly convex Banach spaces — the form used in optimization

## Riesz representation — the dual is the space

$$\boxed{\;\forall f \in H^*\ \exists!\, z_f \in H: \ f(x) = \langle z_f, x \rangle \ \forall x, \qquad \|f\| = \|z_f\|\;}$$

*Mechanism:* $\ker(f)$ is a closed hyperplane ⇒ $\ker(f)^\perp$ is one-dimensional; scale its generator. The map $f \mapsto z_f$ is an **anti-linear isometric bijection** $H^* \cong H$ (quantum mechanics' bra–ket: $|\psi\rangle \leftrightarrow \langle\psi|$). Immediate payoffs: $H$ **reflexive**; weak convergence internal — $x_n \rightharpoonup x \Leftrightarrow \langle y, x_n - x \rangle \to 0\ \forall y \in H$; **every bounded sequence has a weakly convergent subsequence** (via [[Dual Spaces and Weak Topologies|Banach–Alaoglu]]) — the cleanest compactness statement in the course.

## Orthonormal bases — Fourier analysis abstracted

**ONB** = *maximal* orthonormal set ($\|e_i\| = 1$, $\langle e_i, e_j \rangle = 0$ for $i \neq j$). Three equivalent tests:
$$S \text{ maximal} \iff \big(\langle x, e \rangle = 0\ \forall e \in S \Rightarrow x = 0\big) \iff \overline{\operatorname{span}}(S) = H$$

- **Bessel** (any orthonormal family): $\sum_{i \in I} |\langle e_i, x \rangle|^2 \le \|x\|^2$ — coefficients always square-summable; at most countably many nonzero even for uncountable $I$
- **Parseval** (ONB — the equality):
$$x = \sum_{i \in I} \langle e_i, x \rangle\, e_i \ \text{ (unconditionally)}, \qquad \|x\|^2 = \sum_{i \in I} |\langle e_i, x \rangle|^2$$
  Generalized Pythagoras; for trigonometric ONBs this *is* the theory of Fourier series
- Every Hilbert space has an ONB (Zorn); separable ⇔ every ONB countable. **Classification**: $\dim H = \infty$ separable ⇒ $H \cong \ell^2$ via $x \mapsto (\langle e_i, x \rangle)_{i \in \mathbb{N}}$ — *"there is essentially only one Hilbert space."* The interesting structure is never the space; it's the operators on it
- Fine print: unconditional ≠ absolute convergence in infinite dimensions (Dvoretzky–Rogers: every infinite-dimensional Banach space has unconditionally-but-not-absolutely convergent series)

## Adjoints — the $*$ operation

Via Riesz, the [[Normed and Banach Spaces|dual operator]] $T': f \mapsto f \circ T$ pulls back to the space itself:
$$T^* \in BL(H) \text{ defined by } \langle x, Ty \rangle = \langle T^* x, y \rangle \ \forall x,y, \qquad \|T^*\| = \|T\|$$

- Calculus: $(ST)^* = T^* S^*$, $T^{**} = T$, $(\lambda T)^* = \bar{\lambda} T^*$, and the **C\*-identity** $\|T^* T\| = \|T\|^2$ (the seed of C*-algebra theory). Structural duality — the abstract rank–nullity, used verbatim in [[Spectral Theory|Fredholm theory]]:
$$\ker S = (\operatorname{range} S^*)^\perp, \qquad \ker S^* = (\operatorname{range} S)^\perp$$
- Zoo: **self-adjoint** $T^* = T$; **unitary** $T^* T = T T^* = I$ (the isomorphisms); **normal** $T^* T = T T^*$. Matrix case: $* $ = conjugate transpose. Shift on $\ell^2$: $S(x_1, x_2, \dots) = (x_2, x_3, \dots)$, $S^*(x_1, x_2, \dots) = (0, x_1, x_2, \dots)$; $S S^* = I$ but $S^* S = P_{\{x_1 = 0\}}$ — not normal
- **Hellinger–Toeplitz**: $\langle Tx, y \rangle = \langle x, Ty \rangle\ \forall x,y$ with $\operatorname{dom}(T) = H$ ⇒ $T$ bounded (closed graph theorem). Deep consequence: unbounded self-adjoint operators — position, momentum, every Hamiltonian — *cannot* be everywhere-defined; domains are forced by the mathematics, not pedantry
- Self-adjoint $T$: $\|T\| = \sup_{\|x\|=1} |\langle x, Tx \rangle|$ — the norm is visible on the diagonal; the lever that makes the [[Spectral Theory|spectral theorem]] work

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
