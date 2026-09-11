---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [spectrum, compact operator, spectral theorem, Fredholm alternative, resolvent]
---

# Spectral Theory

> **Where this sits.** Chapter 6 of [[18.102 Functional Analysis]] — the payoff. Finite-dimensional linear algebra diagonalizes symmetric matrices; this chapter recovers that in infinite dimensions, and locates exactly where it survives intact: **compact self-adjoint operators**. Throughout: $X$ a *complex* Banach space, $A \in BL(X)$, write $A - \lambda$ for $A - \lambda I$ (complex analysis is a working tool here, not a convenience).

## The spectrum — eigenvalues, corrected for infinite dimensions

If $\dim X = \infty$, $A - \lambda$ can be injective yet non-invertible — "eigenvalue" is too small a notion. Define
$$\rho(A) = \{\lambda \in \mathbb{C} : (A - \lambda)^{-1} \in BL(X)\} \ \text{(resolvent set)}, \qquad \sigma(A) = \mathbb{C} \setminus \rho(A) \ \text{(spectrum)}$$
split by *how* invertibility fails:

| part | $A - \lambda$ is | typical shape |
|---|---|---|
| **point** $\sigma_p$ | not injective — $\ker(A - \lambda) \neq \{0\}$: **eigenvalues** | isolated points |
| **continuous** $\sigma_c$ | injective, not surjective, $\overline{\operatorname{range}} = X$ | intervals |
| **residual** $\sigma_r$ | injective, $\overline{\operatorname{range}} \neq X$ | $\emptyset$ for self-adjoint |

The basic facts, each with its mechanism:

- **Neumann series**: $\|A\| < 1 \Rightarrow (I - A)^{-1} = \sum_{n=0}^{\infty} A^n \in BL(X)$ — the geometric series, verbatim. Everything below flows from it
- $\rho(A)$ **open**, $\lambda \mapsto R_\lambda(A) = (A - \lambda)^{-1}$ **analytic**: perturb an invertible operator, $R_\lambda = \sum_n (\lambda_0 - \lambda)^n R_{\lambda_0}^{n+1}$
- $\sigma(A)$ **compact, non-empty**, $\sigma(A) \subset \{|z| \le \|A\|\}$ ($|\lambda| > \|A\|$ ⇒ Neumann inverts $\lambda(\tfrac{A}{\lambda} - I)$). Non-emptiness is pure complex analysis: $\rho(A) = \mathbb{C}$ would make $\lambda \mapsto f(R_\lambda(A))$ entire and $\to 0$, hence $\equiv 0$ by **Liouville** — contradicting Hahn–Banach. (This is why the field is $\mathbb{C}$)
- **Spectral radius formula**:
$$r(A) = \lim_{n \to \infty} \|A^n\|^{1/n} = \max\{|\lambda| : \lambda \in \sigma(A)\}$$
  For **normal** operators on Hilbert space the C\*-identity upgrades it to $r(A) = \|A\|$ — the norm *is* spectral data
- Self-adjoint ⇒ $\sigma(A) \subset \overline{W(A)} \cap \mathbb{R}$ (numerical range $W(A) = \{\langle x, Ax \rangle : \|x\| = 1\}$) and $\sigma_r = \emptyset$ (via $\overline{\operatorname{range}(A - \lambda)}^\perp = \ker(A - \bar{\lambda})$). Instructive example: the Volterra operator $Af(x) = \int_0^x f$ on $C[0,1]$ has $\sigma(A) = \{0\}$ — quasinilpotent; the spectrum can tell you *nothing pointwise*

## Compact operators — the finite-dimensional shadow

$$K \in BL(X,Y) \text{ compact} :\iff \overline{K(B)} \text{ compact } \forall B \text{ bounded} \iff \forall (x_n) \text{ bdd } \exists (K x_{n_k}) \text{ norm-convergent}$$

equivalently: $x_n \rightharpoonup x \Rightarrow K x_n \to K x$ — compact operators convert [[Dual Spaces and Weak Topologies|weak convergence]] into norm convergence, *smoothing away* the infinite-dimensional pathology.

- $K(X)$ is a **closed, two-sided, $*$-closed ideal** of $BL(H)$: $\|T_n - T\| \to 0$, $T_n$ compact ⇒ $T$ compact; $TK, KT$ compact for any bounded $T$; $K$ compact ⇔ $K^*$ compact (Schauder). $\dim \operatorname{range} < \infty$ ⇒ compact; on Hilbert spaces every compact operator is a norm-limit of finite-rank ones (in general Banach spaces this is the *approximation property* — false, Enflo 1973, who collected the promised live goose)
- The examples that matter: **integral operators** $(T_k f)(x) = \int_0^1 k(x,y) f(y) \, dy$, $k$ continuous — compact via **Arzelà–Ascoli** ($M \subset C(K)$ relatively compact ⇔ pointwise bounded + uniformly equicontinuous — the chapter's real-analysis workhorse); **diagonal operators** $T e_i = \alpha_i e_i$ with $\alpha_i \to 0$ (norm-limit of truncations, $\|T - T_n\| \le \sup_{j \ge n} |\alpha_j|$) — which the spectral theorem says is *the general case*

## The spectral theorem — diagonalization regained

Assembly, one lemma at a time ($T$ compact self-adjoint; each mechanism one line):

1. $\|T\|$ or $-\|T\|$ is an **eigenvalue**: pick $\|x_n\| = 1$ with $|\langle x_n, T x_n \rangle| \to \|T\|$; then $\|T x_n - \lambda x_n\|^2 \le 2\lambda^2 - 2\lambda \langle x_n, T x_n \rangle \to 0$ ($\lambda = \pm\|T\|$), and compactness turns $(T x_n)$ into a convergent subsequence, yielding $Ty = \lambda y$, $y \neq 0$. This is where existence — the hard part in infinite dimensions — gets bought
2. $Tx = \mu x,\ Ty = \lambda y,\ \mu \neq \lambda \Rightarrow \langle x, y \rangle = 0$; eigenvalues are real, form a finite or countable set with only accumulation point $0$ (infinitely many $|\lambda_i| \ge \varepsilon$ would give $\|T x_i - T x_j\|^2 = \lambda_i^2 + \lambda_j^2 \ge 2\varepsilon^2$ — no convergent subsequence); $\dim \ker(T - \lambda) < \infty$ for $\lambda \neq 0$ (its unit ball is compact)
3. $T(M) \subset M \Rightarrow T^*(M^\perp) \subset M^\perp$ — so peel off eigenspaces and recurse on the remainder; if $T|_{\text{remainder}} \neq 0$, step 1 finds one more eigenvector — contradiction

$$\boxed{\;T x = \sum_n \lambda_n \langle e_n, x \rangle\, e_n, \qquad H = \ker(T) \oplus \overline{\operatorname{span}}(e_1, e_2, \dots), \qquad \lambda_n \to 0,\ \ \|T\| = \sup_n |\lambda_n|\;}$$

Every compact self-adjoint operator **is** a diagonal operator in the right [[Hilbert Spaces|orthonormal basis]] — the symmetric-matrix theorem with compactness playing the role finite-dimensionality played. (Beyond compactness lies the general spectral theorem with continuous spectrum — not in this course. The invariant subspace problem — does every $T \in BL(H)$ have a closed invariant $M \neq \{0\}, H$? — remains open for Hilbert spaces.)

## The Fredholm alternative — solving $(T - \lambda)x = y$

For $T$ compact, $\lambda \neq 0$: $S = T - \lambda$ satisfies $\dim \ker(S) < \infty$, $\operatorname{range}(S)$ closed, and (via $\ker S = (\operatorname{range} S^*)^\perp$) **index zero**:
$$\operatorname{ind}(S) = \dim \ker(S) - \operatorname{codim} \operatorname{range}(S) = 0 \quad\Longrightarrow\quad S \text{ injective} \iff S \text{ surjective}$$

The **alternative**, as a solvability statement for $(T - \lambda)x = y$:

> **Either** $T - \lambda$ is invertible in $BL(H)$ — unique solution for every $y$ — **or** $\lambda \in \sigma_p(T)$, in which case solutions exist iff $y \perp \ker(T^* - \bar{\lambda})$, unique up to the finite-dimensional $\ker(T - \lambda)$.

In slogan form: **for compact perturbations of the identity, uniqueness ⇔ existence** — exactly as for square matrices. This is the classical existence theory for integral equations (Fredholm's original problem) and, one Sobolev space later, for elliptic boundary-value problems: check the homogeneous equation has only $0$, conclude the inhomogeneous one is uniquely solvable.

## Related

- [[Hilbert Spaces]] — adjoints, the C*-identity, the self-adjoint norm formula
- [[Normed and Banach Spaces]] — inverse mapping theorem (behind the spectrum's trichotomy)
- [[Dual Spaces and Weak Topologies]] — weak convergence, which compact operators upgrade
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 6

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
