---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [spectrum, compact operator, spectral theorem, Fredholm alternative, resolvent]
---

# Spectral Theory

> **Where this sits.** Chapter 6 of [[18.102 Functional Analysis]] — the payoff. Finite-dimensional linear algebra diagonalizes symmetric matrices; this chapter recovers that in infinite dimensions, and locates exactly where it survives intact: **compact self-adjoint operators**. Throughout: $X$ a *complex* Banach space, $A \in BL(X)$, write $A - \lambda$ for $A - \lambda I$ (complex analysis is a working tool here, not a convenience). Every entry: *Idea* → *Formally* → *Mechanism*.

## The spectrum — eigenvalues, corrected for infinite dimensions

**Resolvent set and spectrum.** *Idea:* in infinite dimensions an operator can be injective yet still not invertible, so "eigenvalue" is too coarse — the honest question is: for which $\lambda$ does $A - \lambda$ fail to have a bounded inverse? Those $\lambda$ form the spectrum, and the *manner* of failure sorts it into three parts. *Formally:*
$$\rho(A) = \{\lambda \in \mathbb{C} : (A - \lambda)^{-1} \in BL(X)\}, \qquad \sigma(A) = \mathbb{C} \setminus \rho(A) = \sigma_p \sqcup \sigma_c \sqcup \sigma_r$$

| part | $A - \lambda$ is | typical shape |
|---|---|---|
| **point** $\sigma_p$ | not injective — $\ker(A - \lambda) \neq \{0\}$: **eigenvalues** | isolated points |
| **continuous** $\sigma_c$ | injective, not surjective, $\overline{\operatorname{range}} = X$ | intervals |
| **residual** $\sigma_r$ | injective, $\overline{\operatorname{range}} \neq X$ | $\emptyset$ for self-adjoint |

**Neumann series.** *Idea:* the geometric series works for operators — anything within distance 1 of the identity is invertible, with inverse given by the series. Everything below flows from this. *Formally:* $\|A\| < 1 \Rightarrow (I - A)^{-1} = \sum_{n=0}^{\infty} A^n \in BL(X)$. *Mechanism:* $\|A^n\| \le \|A\|^n$ makes the series absolutely convergent; telescoping.

**The resolvent is open and analytic.** *Idea:* invertibility is stable — perturbing an invertible operator slightly keeps it invertible — so the resolvent set is open and the inverse depends analytically on $\lambda$. *Formally:* $\rho(A)$ open; $\lambda \mapsto R_\lambda(A) = (A - \lambda)^{-1}$ is given locally by convergent power series $R_\lambda = \sum_n (\lambda_0 - \lambda)^n R_{\lambda_0}^{n+1}$. *Mechanism:* factor $A - \lambda = (A - \lambda_0)(I + (\lambda_0 - \lambda) R_{\lambda_0})$, Neumann on the bracket.

**The spectrum is compact and non-empty.** *Idea:* the spectrum lives inside the disk of radius $\|A\|$ (far away, Neumann inverts), and it can't be empty — an empty spectrum would make the resolvent a bounded entire function, which Liouville forbids. This is why the field must be $\mathbb{C}$. *Formally:* $\sigma(A)$ compact, $\neq \emptyset$, $\sigma(A) \subset \{|z| \le \|A\|\}$. *Mechanism:* if $\rho(A) = \mathbb{C}$, then $\lambda \mapsto f(R_\lambda(A))$ is entire and $\to 0$ as $|\lambda| \to \infty$ for every $f \in BL(X)^*$, hence $\equiv 0$ (Liouville); Hahn–Banach then forces $R_\lambda = 0$ — absurd.

**Spectral radius formula.** *Idea:* how far the spectrum reaches is computable from norms of powers — and for normal operators on Hilbert space it equals the norm itself: the norm *is* spectral data. *Formally:*
$$r(A) = \lim_{n \to \infty} \|A^n\|^{1/n} = \max\{|\lambda| : \lambda \in \sigma(A)\}; \qquad A \text{ normal (Hilbert)} \Rightarrow r(A) = \|A\|$$
*Mechanism:* one direction via Neumann convergence of $\sum (A/\lambda)^n$; the other via Laurent expansion of the resolvent + uniform boundedness; the normal case by iterating the C*-identity $\|A^2\| = \|A\|^2$.

**Self-adjoint operators.** *Formally:* $T = T^*$ ⇒ $\sigma(T) \subset \overline{W(T)} \cap \mathbb{R}$, where $W(T) = \{\langle x, Tx \rangle : \|x\| = 1\}$ is the numerical range; and $\sigma_r(T) = \emptyset$ (*mechanism:* $\overline{\operatorname{range}(T - \lambda)}^{\perp} = \ker(T - \bar{\lambda})$, so injectivity forces dense range). Instructive example: the Volterra operator $Af(x) = \int_0^x f$ on $C[0,1]$ has $\sigma(A) = \{0\}$ — quasinilpotent; a spectrum can tell you *nothing pointwise*.

## Compact operators — the finite-dimensional shadow

**Compact operator.** *Idea:* an operator that crushes bounded sets into almost-compact ones — the closest thing infinite dimensions has to a finite-rank map. Equivalent smoothing property: it upgrades weak convergence to norm convergence. *Formally:*
$$K \text{ compact} :\iff \overline{K(B)} \text{ compact } \forall B \text{ bounded} \iff \forall (x_n) \text{ bdd } \exists (K x_{n_k}) \text{ convergent} \;\Rightarrow\; \big(x_n \rightharpoonup x \Rightarrow K x_n \to K x\big)$$

**The ideal structure.** *Idea:* compactness is contagious in every direction — through limits, through composition with anything bounded, through adjoints — so the compact operators form a closed two-sided $*$-ideal: the "small operators" of $BL(H)$. *Formally:* $\|T_n - T\| \to 0$, $T_n$ compact ⇒ $T$ compact; $TK, KT$ compact for bounded $T$; $K$ compact ⇔ $K^*$ compact (Schauder). Finite rank ($\dim \operatorname{range} < \infty$) ⇒ compact; on Hilbert spaces every compact operator is a norm-limit of finite-rank ones (in general Banach spaces this is the *approximation property* — false, Enflo 1973, who collected the promised live goose).

**Arzelà–Ascoli.** *Idea:* the concrete compactness criterion behind the examples — in $C(K)$, a family is relatively compact iff it is bounded and *equally* continuous, so no wiggling escapes to the limit. *Formally:* $M \subset C(K)$, $K$ compact metric: $\overline{M}$ compact ⇔ $M$ pointwise bounded + uniformly equicontinuous ($\forall \varepsilon\ \exists \delta: d(x,y) < \delta \Rightarrow |f(x) - f(y)| \le \varepsilon\ \forall f \in M$). *Mechanism:* diagonal subsequence on a countable dense set, equicontinuity upgrades to uniform convergence.

**The examples that matter.** *Formally:* **integral operators** $(T_k f)(x) = \int_0^1 k(x,y) f(y)\,dy$ with continuous kernel — compact via Arzelà–Ascoli; **diagonal operators** $T e_i = \alpha_i e_i$ with $\alpha_i \to 0$ — compact as norm-limits of truncations, $\|T - T_n\| \le \sup_{j \ge n} |\alpha_j|$. The spectral theorem below says the diagonal example is *the general case*.

## The spectral theorem — diagonalization regained

*Idea of the whole section:* for compact self-adjoint operators, the finite-dimensional symmetric-matrix theorem survives verbatim — an orthonormal system of eigenvectors carrying real eigenvalues that fade to zero, diagonalizing the operator. Compactness plays the role finite-dimensionality played: it buys *existence* of eigenvectors. Assembly in three lemmas:

**Lemma 1 — an extreme eigenvalue exists.** *Idea:* the largest possible Rayleigh quotient is actually *attained* — compactness turns a maximizing sequence into an eigenvector. This is where existence, the hard part in infinite dimensions, gets bought. *Formally:* $T$ compact self-adjoint ⇒ $\|T\|$ or $-\|T\|$ is an eigenvalue. *Mechanism:* pick $\|x_n\| = 1$ with $|\langle x_n, T x_n \rangle| \to \|T\|$ (possible by the [[Hilbert Spaces|self-adjoint norm formula]]); then $\|T x_n - \lambda x_n\|^2 \le 2\lambda^2 - 2\lambda \langle x_n, T x_n \rangle \to 0$ for $\lambda = \pm\|T\|$; compactness extracts $T x_{n_k} \to$ some $y \neq 0$ with $Ty = \lambda y$.

**Lemma 2 — the eigenvalue structure.** *Formally:* eigenvectors of distinct eigenvalues are orthogonal ($\mu \langle x,y \rangle = \langle Tx, y \rangle = \lambda \langle x,y \rangle$); eigenvalues are real, at most countably many, accumulating only at $0$ (*mechanism:* infinitely many $|\lambda_i| \ge \varepsilon$ give orthonormal eigenvectors with $\|T x_i - T x_j\|^2 = \lambda_i^2 + \lambda_j^2 \ge 2\varepsilon^2$ — no convergent subsequence); each $\ker(T - \lambda)$, $\lambda \neq 0$, is finite-dimensional (its unit ball is compact).

**Lemma 3 — peel and recurse.** *Formally:* $T(M) \subset M \Rightarrow T^*(M^\perp) \subset M^\perp$; so remove the closed span of all eigenvectors, restrict $T$ to the orthogonal complement (still compact self-adjoint), and if the restriction were nonzero, Lemma 1 would produce an eigenvector *outside* the span — contradiction.

$$\boxed{\;T x = \sum_n \lambda_n \langle e_n, x \rangle\, e_n, \qquad H = \ker(T) \oplus \overline{\operatorname{span}}(e_1, e_2, \dots), \qquad \lambda_n \to 0,\ \ \|T\| = \sup_n |\lambda_n|\;}$$

(Beyond compactness lies the general spectral theorem with continuous spectrum — not in this course. The invariant subspace problem — does every $T \in BL(H)$ have a closed invariant subspace $\neq \{0\}, H$? — remains open for Hilbert spaces.)

## The Fredholm alternative — solving $(T - \lambda)x = y$

**The structure theorem.** *Idea:* subtracting the identity from a compact operator produces something *almost invertible*: kernel finite-dimensional, range closed and of matching finite codimension — so injectivity and surjectivity, wildly independent in general, become equivalent again, exactly as for square matrices. *Formally:* $T$ compact, $\lambda \neq 0$, $S = T - \lambda$:
$$\dim \ker(S) < \infty, \quad \operatorname{range}(S) \text{ closed}, \quad \operatorname{ind}(S) = \dim \ker(S) - \operatorname{codim} \operatorname{range}(S) = 0 \;\Longrightarrow\; S \text{ injective} \iff S \text{ surjective}$$
*Mechanism:* on $\ker(S)$, $x = \tfrac{1}{\lambda} Tx$, so its unit ball is compact (finite-dim by [[Normed and Banach Spaces|the dividing line]]); closed range because $S$ is bounded below off its kernel (else compactness manufactures a kernel vector of quotient-norm 1); the codimension count via $\operatorname{range}(S)^\perp = \ker(S^*)$, same argument for $T^*$; injective-but-not-surjective would make $\operatorname{range}(S^j)$ a strictly decreasing chain of closed subspaces, contradicting compactness.

**The alternative, as a solvability statement.** *Idea:* for the equation $(T - \lambda)x = y$, exactly one of two worlds holds: either it is uniquely solvable for *every* right-hand side, or $\lambda$ is an eigenvalue — and then solvability is a finite list of orthogonality conditions on $y$. "Uniqueness implies existence." *Formally:*

> **Either** $T - \lambda$ is invertible in $BL(H)$ — unique solution $\forall y$ — **or** $\lambda \in \sigma_p(T)$, and then $(T - \lambda)x = y$ is solvable iff $y \perp \ker(T^* - \bar{\lambda})$, uniquely up to the finite-dimensional $\ker(T - \lambda)$.

This is the classical existence theory for integral equations (Fredholm's original problem) and, one Sobolev space later, for elliptic boundary-value problems: check the homogeneous equation has only $0$, conclude the inhomogeneous one is uniquely solvable.

## Related

- [[Hilbert Spaces]] — adjoints, the C*-identity, the self-adjoint norm formula
- [[Normed and Banach Spaces]] — inverse mapping theorem (behind the spectrum's trichotomy)
- [[Dual Spaces and Weak Topologies]] — weak convergence, which compact operators upgrade
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 6

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
