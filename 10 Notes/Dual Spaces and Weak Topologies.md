---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [weak topology, weak-star topology, Banach-Alaoglu, Lp duality, weak convergence]
---

# Dual Spaces and Weak Topologies

> **Where this sits.** Chapter 4 of [[18.102 Functional Analysis]] — the compactness rescue. [[Normed and Banach Spaces|Ch. 2's dividing line]]: $\dim X = \infty \Rightarrow \overline{B}_1(0)$ never norm-compact, so minimizing sequences have no convergent subsequences and existence proofs die. The fix: **make the topology coarser** — fewer open sets means more compact sets — exactly as coarse as the dual allows. Payoff: Banach–Alaoglu, and the explicit duals of $L^p$. Every entry: *Idea* → *Formally* → *Mechanism*.

## The topologies

**Locally convex space.** *Idea:* a vector space topologized not by one norm but by a *family* of seminorms — a point is close to another if all the seminorms say so; the family only needs to jointly distinguish points. *Formally:* a TVS whose topology has neighborhood subbase $\{y : p_i(x - y) < \varepsilon\}$ for a family $(p_i)_{i \in I}$ of seminorms that **separates points**: $x \neq 0 \Rightarrow \exists i: p_i(x) \neq 0$ (forces Hausdorff).

**Weak and weak-\* topologies.** *Idea:* on $X$, keep only the open sets the dual's functionals can detect — convergence means "every measurement converges." On $X^*$, be even stingier: test only against points of $X$ itself, not all of $X^{**}$ — pointwise convergence of functionals. Both are [[Topological and Metric Spaces|initial topologies]]. *Formally:*

| topology | on | seminorms | = initial topology of | convergence |
|---|---|---|---|---|
| **weak** $\sigma(X, X^*)$ | $X$ | $x \mapsto \lvert f(x) \rvert$, $f \in X^*$ | all $f \in X^*$ | $x_n \rightharpoonup x :\Leftrightarrow f(x_n) \to f(x)\ \forall f \in X^*$ |
| **weak-*** $\sigma(X^*, X)$ | $X^*$ | $f \mapsto \lvert f(x) \rvert$, $x \in X$ | all evaluations $i(x)$, $x \in X$ | $f_n \stackrel{*}{\rightharpoonup} f :\Leftrightarrow f_n(x) \to f(x)\ \forall x \in X$ |

Order of fineness: $\sigma(X, X^*) \subset \mathcal{T}_{\|\cdot\|}$ (equal iff $\dim X < \infty$); on $X^*$: $\sigma(X^*, X) \subset \sigma(X^*, X^{**})$, equal iff $X$ reflexive. Weak-* is the *cheapest* topology on the dual — exactly why its compactness theorem is the strongest. In infinite dimensions neither is metrizable on the whole space (on bounded balls weak-* *is*, when $X$ is separable — the loophole below).

**What weak limits preserve.** *Idea:* a weakly convergent sequence can't be wild — it is automatically bounded — and the norm can only *drop* in the limit: mass may leak away, never appear. *Formally:* $x_n \rightharpoonup x \Rightarrow \sup_n \|x_n\| < \infty$ (uniform boundedness in disguise) and $\|x\| \le \liminf_n \|x_n\|$ (weak lower semicontinuity of the norm).

**Schur property.** *Idea:* in $\ell^1$ the dual is so large that weak convergence of *sequences* already forces norm convergence — yet the topologies still differ; sequences don't see everything. *Formally:* in $\ell^1$: $x_n \rightharpoonup x \Rightarrow \|x_n - x\|_1 \to 0$.

## Banach–Alaoglu — compactness regained

**Banach–Alaoglu theorem.** *Idea:* the dual's unit ball, hopelessly non-compact in norm, becomes compact the moment you only demand pointwise convergence — because pointwise, a bounded functional is just one coordinate per point of $X$, each ranging in a compact disk, and Tychonoff makes the product of all those disks compact. *Formally:*
$$\boxed{\;\overline{B}_{X^*} = \{f \in X^* : \|f\| \le 1\} \text{ is weak-* compact, for every normed } X\;}$$
*Mechanism:* $\|f\| \le 1 \Rightarrow f(x) \in K_x = \{|\lambda| \le \|x\|\}$, so $\overline{B}_{X^*} \subset \prod_{x \in X} K_x$, compact by [[Topological and Metric Spaces|Tychonoff]]; the weak-* topology *is* the restricted product topology (both initial topologies of the evaluations); linearity is a closed condition — $\overline{B}_{X^*} = \bigcap_{x,y,\lambda,\mu} \{f : (\pi_{\lambda x + \mu y} - \lambda \pi_x - \mu \pi_y)(f) = 0\}$. Closed subset of compact ⇒ compact. □

**Sequential version.** *Idea:* when $X$ is separable, compactness comes in the usable form: every bounded sequence of functionals has a pointwise-convergent subsequence. *Formally:* $X$ separable ⇒ $\overline{B}_{X^*}$ weak-* sequentially compact. *Mechanism:* the ball is weak-* metrizable (or directly: diagonal argument over a countable dense set + bounded linear extension).

**What it's for.** *Idea:* bounded sequence → weak-* cluster point, for free. In reflexive spaces the same holds for the weak topology on $X$ itself — bounded sequences in $L^p$ ($1 < p < \infty$) or [[Hilbert Spaces|Hilbert space]] have weakly convergent subsequences. This is the engine of the **direct method of the calculus of variations**: bounded minimizing sequence → weak limit → lower semicontinuity of the functional → the limit is the minimizer. Caution: only the *ball* is compact — $\operatorname{int}_{w*}(\overline{B}_{X^*}) = \emptyset$; the space is not locally compact.

## Riesz representation for $L^p$ — the duals, explicitly

**The theorem.** *Idea:* every continuous functional on $L^p$ is "integrate against a fixed function of the conjugate power," and the correspondence preserves norms exactly — the dual of $L^p$ *is* $L^q$, concretely. *Formally:* with $\tfrac1p + \tfrac1q = 1$ and $\phi_g(f) = \int_\Omega g f \, d\mu$:
$$\boxed{\;(L^p)^* \cong L^q \text{ isometrically},\ 1 < p < \infty; \qquad (L^1)^* \cong L^\infty \ (\mu\ \sigma\text{-finite})\;}$$
*Mechanism:* $\|\phi_g\| \le \|g\|_q$ is Hölder; equality by testing against $f = e^{i\theta}|g|^{q-1}$ — the equality case of Hölder used as a construction. Surjectivity, this course's route (no Radon–Nikodym): given $F$ with $\|F\| = 1$, any sequence $\|f_n\|_p = 1$, $F(f_n) \to 1$ is Cauchy **by [[Measure Theory and the Lebesgue Integral|uniform convexity]]** — the maximizer $f$ exists and $g = \lambda |f|^{p/q}$ represents $F$; the case $p = 1$ follows by gluing over finite-measure pieces.

**Consequences.** *Formally:* $L^p$ reflexive for $1 < p < \infty$. NOT reflexive: $\ell^1, \ell^\infty, L^1, L^\infty$. The asymmetry to remember: $(L^1)^* = L^\infty$ but $(L^\infty)^* \supsetneq L^1$. *Mechanism for the last:* extend $f \mapsto f(0)$ from $C_b$ to $L^\infty$ by Hahn–Banach — a "delta function" no $L^1$-function represents.

## Related

- [[Normed and Banach Spaces]] — duals, reflexivity, Hahn–Banach (used twice here)
- [[Measure Theory and the Lebesgue Integral]] — $L^p$, uniform convexity
- [[Hilbert Spaces]] — where the dual is the space itself and weak compactness is cleanest
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 4

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
