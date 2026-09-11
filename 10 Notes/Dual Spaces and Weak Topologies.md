---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [weak topology, weak-star topology, Banach-Alaoglu, Lp duality, weak convergence]
---

# Dual Spaces and Weak Topologies

> **Where this sits.** Chapter 4 of [[18.102 Functional Analysis]] — the compactness rescue. [[Normed and Banach Spaces|Ch. 2's dividing line]]: $\dim X = \infty \Rightarrow \overline{B}_1(0)$ never norm-compact, so minimizing sequences have no convergent subsequences and existence proofs die. The fix: **make the topology coarser** — fewer open sets, more compact sets — exactly as coarse as the dual allows. Payoff: Banach–Alaoglu, and the explicit duals of $L^p$.

## The topologies

Machinery: a **locally convex space** = TVS whose topology is induced by a separating family of seminorms $(p_i)_{i \in I}$ (neighborhood subbase $\{y : p_i(x - y) < \varepsilon\}$; separating: $x \neq 0 \Rightarrow \exists i: p_i(x) \neq 0$, which forces Hausdorff). Both weak topologies are instances, and both are [[Topological and Metric Spaces|initial topologies]]:

| topology | on | seminorms | = initial topology of | convergence |
|---|---|---|---|---|
| **weak** $\sigma(X, X^*)$ | $X$ | $x \mapsto \lvert f(x) \rvert$, $f \in X^*$ | all $f \in X^*$ | $x_n \rightharpoonup x :\Leftrightarrow f(x_n) \to f(x)\ \forall f \in X^*$ |
| **weak-*** $\sigma(X^*, X)$ | $X^*$ | $f \mapsto \lvert f(x) \rvert$, $x \in X$ | all evaluations $i(x)$, $x \in X$ | $f_n \stackrel{*}{\rightharpoonup} f :\Leftrightarrow f_n(x) \to f(x)\ \forall x \in X$ |

- $\sigma(X, X^*) \subset \mathcal{T}_{\|\cdot\|}$, equal iff $\dim X < \infty$. On $X^*$: $\sigma(X^*, X) \subset \sigma(X^*, X^{**})$, equal iff $X$ reflexive — weak-* tests only against $X$, not $X^{**}$: the *cheapest* topology on the dual, which is exactly why its compactness theorem is strongest
- $\dim X = \infty$ ⇒ neither topology is metrizable on the whole space (though on bounded balls weak-* *is*, when $X$ separable — the loophole below)
- Weak convergence facts: $x_n \rightharpoonup x \Rightarrow \sup_n \|x_n\| < \infty$ (uniform boundedness in disguise) and $\|x\| \le \liminf_n \|x_n\|$ (norm weakly lower semicontinuous) — mass can leak in a weak limit, never appear
- Curiosity with content: $\ell^1$ has the **Schur property** — $x_n \rightharpoonup x \Rightarrow \|x_n - x\|_1 \to 0$ (the dual $\ell^\infty$ is that large) — yet the topologies still differ

## Banach–Alaoglu — compactness regained

$$\boxed{\;\overline{B}_{X^*} = \{f \in X^* : \|f\| \le 1\} \text{ is weak-* compact, for every normed } X\;}$$

*Mechanism, worth remembering:* $\|f\| \le 1$ means $f(x) \in K_x = \{\lambda : |\lambda| \le \|x\|\}$ for every $x$, so $\overline{B}_{X^*} \subset \prod_{x \in X} K_x$ — compact by [[Topological and Metric Spaces|Tychonoff]]; the weak-* topology *is* the product topology restricted there (both are initial topologies of the evaluations); and $\overline{B}_{X^*} = \{f \in K : f \text{ linear}\} = \bigcap_{x,y,\lambda,\mu} \{f : (\pi_{\lambda x + \mu y} - \lambda \pi_x - \mu \pi_y)(f) = 0\}$ is closed in the product. Closed ⊂ compact ⇒ compact. □

**Separable case**: $X$ separable ⇒ $\overline{B}_{X^*}$ weak-* **sequentially** compact — every bounded $(f_n) \subset X^*$ has a weak-*-convergent subsequence (metrizability of the ball, or directly: diagonal argument over a countable dense set + bounded linear extension).

Why this is *the* existence tool: bounded sequence → weak-* cluster point, for free. In reflexive spaces the same holds for the weak topology on $X$ itself — bounded sequences in $L^p$ ($1<p<\infty$) or [[Hilbert Spaces|Hilbert space]] have weakly convergent subsequences. This is the engine of the **direct method of the calculus of variations**: bounded minimizing sequence → weak limit → lower semicontinuity. Caution: only the *ball* is compact — $\operatorname{int}_{w*}(\overline{B}_{X^*}) = \emptyset$; the space is not locally compact.

## Riesz representation for $L^p$ — the duals, explicitly

With $\tfrac1p + \tfrac1q = 1$, the pairing $\phi: L^q \to (L^p)^*$, $\phi_g(f) = \int_\Omega g f \, d\mu$:

$$\boxed{\;(L^p)^* \cong L^q \text{ isometrically},\ 1 < p < \infty; \qquad (L^1)^* \cong L^\infty \ (\mu\ \sigma\text{-finite})\;}$$

- $\|\phi_g\| \le \|g\|_q$ is Hölder; equality by testing against $f = e^{i\theta} |g|^{q-1}$ (the equality case of Hölder, used as a *construction*)
- Surjectivity, this course's route (elementary, no Radon–Nikodym): given $F \in (L^p)^*$, $\|F\| = 1$, any sequence with $\|f_n\|_p = 1,\ F(f_n) \to 1$ is Cauchy **by uniform convexity** ([[Measure Theory and the Lebesgue Integral|Clarkson]]) — the maximizer $f$ exists, and $g = \lambda |f|^{p/q}$ represents $F$. The case $p = 1$ follows by a limiting/gluing argument over finite-measure pieces
- Consequences: **$L^p$ reflexive for $1 < p < \infty$** (the dual of the dual walks back). NOT reflexive: $\ell^1, \ell^\infty, L^1, L^\infty$. And the asymmetry to remember: $(L^1)^* = L^\infty$ but $(L^\infty)^* \supsetneq L^1$ — e.g. extend $f \mapsto f(0)$ from $C_b$ by Hahn–Banach: a functional on $L^\infty$ no $L^1$-function represents (a "delta function" living in the bidual)

## Related

- [[Normed and Banach Spaces]] — duals, reflexivity, Hahn–Banach (used twice here)
- [[Measure Theory and the Lebesgue Integral]] — $L^p$, uniform convexity
- [[Hilbert Spaces]] — where the dual is the space itself and weak compactness is cleanest
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 4

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
