---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [spectrum, compact operator, spectral theorem, Fredholm alternative, resolvent]
---

# Spectral Theory

> **Where this sits.** Chapter 6 of [[18.102 Functional Analysis]] — the payoff. Finite-dimensional linear algebra diagonalizes symmetric matrices; this chapter recovers that in infinite dimensions, and locates exactly where it survives intact: **compact self-adjoint operators**. Throughout: $X$ a *complex* Banach space (complex analysis is a working tool here, not a convenience).

## The spectrum — eigenvalues, corrected for infinite dimensions

In infinite dimensions $A - \lambda$ can be injective yet fail to be invertible — so "eigenvalue" is too small a notion. Define the **resolvent set** $\rho(A) = \{\lambda : (A-\lambda)^{-1} \in BL(X)\}$ and the **spectrum** $\sigma(A) = \mathbb{C} \setminus \rho(A)$, split by *how* invertibility fails:

| part | $A - \lambda$ is | typical shape |
|---|---|---|
| **point** $\sigma_p$ | not injective (**eigenvalues**) | isolated points |
| **continuous** $\sigma_c$ | injective, dense range, not surjective | intervals |
| **residual** $\sigma_r$ | injective, range not dense | ∅ for self-adjoint |

The basic facts, each with its mechanism:

- **Neumann series**: $\|A\| < 1 \Rightarrow (I - A)^{-1} = \sum A^n$ — the geometric series, verbatim. Everything below flows from it
- $\rho(A)$ is **open** and $\lambda \mapsto (A-\lambda)^{-1}$ is **analytic** (perturb an invertible operator by a Neumann series)
- $\sigma(A)$ is **compact, non-empty**, contained in the disk $|z| \le \|A\|$. Non-emptiness is pure complex analysis: were the resolvent entire, Liouville would force it constant — contradiction. (This is why the field is $\mathbb{C}$)
- **Spectral radius formula**: $r(A) = \lim \|A^n\|^{1/n} = \max\{|\lambda| : \lambda \in \sigma(A)\}$. For **normal** operators on Hilbert space the C\*-identity upgrades it to $r(A) = \|A\|$ — the norm *is* spectral data
- Self-adjoint ⇒ $\sigma(A) \subset \mathbb{R}$ (inside the closure of the numerical range $\{\langle x, Ax\rangle\}$) and $\sigma_r = \emptyset$. Instructive example: the Volterra operator $f \mapsto \int_0^x f$ on $C[0,1]$ has $\sigma = \{0\}$ — quasinilpotent, spectrum tells you *nothing pointwise*

## Compact operators — the finite-dimensional shadow

$K$ **compact** iff it maps bounded sets to relatively compact ones ⇔ every bounded sequence gets a norm-convergent image subsequence ⇔ (equivalently) $K$ converts [[Dual Spaces and Weak Topologies|weak convergence]] into norm convergence — compact operators *smooth away* the infinite-dimensional pathology.

- $K(X)$ is a **closed, two-sided, $*$-closed ideal** in $BL(H)$: limits of compacts are compact, products with anything bounded stay compact, adjoints stay compact (Schauder). Finite-rank ⇒ compact; on Hilbert spaces every compact operator is a norm-limit of finite-rank ones (in general Banach spaces this is the *approximation property* — false, Enflo 1973, who was paid the promised live goose)
- The examples that matter: **integral operators** $f \mapsto \int k(x,y) f(y)\,dy$ with continuous kernel (via **Arzelà–Ascoli**: bounded + equicontinuous ⇒ relatively compact in $C(K)$ — the chapter's real-analysis workhorse); **diagonal operators** $T e_i = \alpha_i e_i$ with $\alpha_i \to 0$ — which the spectral theorem below says is *the general case*

## The spectral theorem — diagonalization regained

Assembly, one lemma at a time (each mechanism one line):

1. $T$ compact self-adjoint ⇒ $\|T\|$ or $-\|T\|$ is an **eigenvalue** (maximize $|\langle x, Tx\rangle|$; compactness turns the maximizing sequence into an eigenvector — this is where existence, the hard part in infinite dimensions, gets bought)
2. Distinct eigenvalues ⇒ orthogonal eigenvectors; eigenvalues are real, form a finite or countable set accumulating only at $0$; each nonzero eigenspace is finite-dimensional (else the unit ball of an eigenspace violates compactness)
3. Invariant subspace ⇒ orthogonal complement invariant (self-adjointness) — so peel off eigenspaces and recurse on what's left; if anything nonzero remained, step 1 would find one more eigenvector

$$\boxed{\;T x = \sum_n \lambda_n \langle e_n, x\rangle\, e_n, \qquad H = \ker(T) \oplus \overline{\operatorname{span}}(e_1, e_2, \dots), \qquad \lambda_n \to 0\;}$$

Every compact self-adjoint operator **is** a diagonal operator in the right [[Hilbert Spaces|orthonormal basis]] — the infinite-dimensional symmetric-matrix theorem, with compactness playing the role finite-dimensionality played. (Beyond compactness lies the general spectral theorem with continuous spectrum — not in this course. The invariant subspace problem for general operators on Hilbert space remains open.)

## The Fredholm alternative — solving $(T - \lambda)x = y$

For $T$ compact and $\lambda \neq 0$: $T - \lambda$ has finite-dimensional kernel, closed range, and **index zero** ($\dim\ker = \operatorname{codim}\,\mathrm{range}$, via $\ker S = (\operatorname{range} S^*)^\perp$). Consequence, the **alternative**:

> **Either** $(T-\lambda)x = y$ has a *unique* solution for *every* $y$ (i.e. $T - \lambda$ invertible), **or** $\lambda$ is an eigenvalue — in which case solvability holds exactly for $y \perp \ker(T^* - \bar\lambda)$, with finitely many degrees of freedom.

In slogan form: **for compact perturbations of the identity, uniqueness ⇔ existence** — exactly as for square matrices. This is the classical existence theory for integral equations (Fredholm's original problem) and, one Sobolev space later, for elliptic boundary-value problems: check the homogeneous equation has only the zero solution, conclude the inhomogeneous one is uniquely solvable.

## Related

- [[Hilbert Spaces]] — adjoints, the C*-identity, the self-adjoint norm formula
- [[Normed and Banach Spaces]] — inverse mapping theorem (behind the spectrum's trichotomy)
- [[Dual Spaces and Weak Topologies]] — weak convergence, which compact operators upgrade
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 6

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
