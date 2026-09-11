---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [measure theory, Lebesgue integral, Lp spaces, dominated convergence, Riesz-Fischer, Caratheodory]
---

# Measure Theory and the Lebesgue Integral

> **Where this sits.** Chapter 3 of [[18.102 Functional Analysis]] — the longest chapter, and the construction of the course's main *examples*: the $L^p$ spaces. Three reasons Riemann isn't enough, each naming a payoff below: **(1)** integrate over general sets, not just intervals ($1_{\mathbb{Q}}$ becomes integrable); **(2)** completeness — Riemann-integrable functions under $\int|f-g|$ are *not* complete, and $L^1$ is exactly the missing completion; **(3)** convergence theorems — Riemann tolerates only uniform limits, Lebesgue tolerates pointwise-a.e. + domination. (Also: the foundation of modern probability.)

## Measure spaces — the axioms of "volume"

- **σ-algebra** $\mathcal{F} \subset \mathcal{P}(\Omega)$: $\emptyset \in \mathcal{F}$; $A \in \mathcal{F} \Rightarrow A^c \in \mathcal{F}$; $A_i \in \mathcal{F}\ (i \in \mathbb{N}) \Rightarrow \bigcup_i A_i \in \mathcal{F}$. **Measure**: $\mu: \mathcal{F} \to [0,\infty]$, $\mu(\emptyset) = 0$, $\mu\big(\bigsqcup_i A_i\big) = \sum_i \mu(A_i)$ (countable additivity). **Null set**: $\mu(A) = 0$; "**a.e.**" = off a null set — the resolution at which all of $L^p$ theory operates
- **Continuity of measure**: $E_n \uparrow \Rightarrow \mu(\bigcup_n E_n) = \lim_n \mu(E_n)$; $E_n \downarrow$ with some $\mu(E_n) < \infty \Rightarrow \mu(\bigcap_n E_n) = \lim_n \mu(E_n)$ — countable additivity's workhorse form
- $\sigma(\mathcal{A})$ = smallest σ-algebra $\supset \mathcal{A}$ (intersection of all); **Borel** $\mathcal{B} = \sigma(\mathcal{T})$

## Carathéodory's machine — why not all sets are measurable

**Vitali's impossibility**: no $\mu: \mathcal{P}(\mathbb{R}) \to [0,\infty]$ satisfies simultaneously (i) defined on *all* subsets, (ii) $\mu((a,b)) = b - a$, (iii) countable additivity + translation invariance. Something must go — and the right sacrifice is (i): restrict to a σ-algebra of "measurable" sets.

The construction: an **outer measure** $\mu^*: \mathcal{P}(\Omega) \to [0,\infty]$ ($\mu^*(\emptyset) = 0$; monotone; $\mu^*(\bigcup_i A_i) \le \sum_i \mu^*(A_i)$) → **Carathéodory's criterion** picks the good sets:
$$A \text{ measurable} \;:\Longleftrightarrow\; \mu^*(B) = \mu^*(B \cap A) + \mu^*(B \cap A^c) \quad \forall B \subset \Omega$$
("$A$ slices *every* set additively") → **Extension theorem**: these sets form a σ-algebra on which $\mu = \mu^*|_{\mathcal{F}}$ is a genuine, *complete* measure ($A \subset N$, $\mu(N) = 0 \Rightarrow A \in \mathcal{F}$).

**Lebesgue measure**: run $\lambda^*(A) = \inf\big\{\sum_i \operatorname{vol}(C_i) : C_i \text{ open cuboids},\ A \subset \bigcup_i C_i\big\}$ through the machine. Facts: $\lambda(C) = \operatorname{vol}(C)$ on cuboids; $\lambda(A + x) = \lambda(A)$; open and closed sets measurable; **regular**: $\lambda(A) = \sup_{K \subset A \text{ cpt}} \lambda(K) = \inf_{O \supset A \text{ open}} \lambda(O)$; Lebesgue σ-algebra $\mathcal{L}$ = completion of Borel ($A \in \mathcal{L} \Leftrightarrow A = \text{Borel} \cup \text{null}$). Non-measurable sets exist but *only via the axiom of choice* — rule of thumb: any set you can write down is measurable. (Banach–Tarski is the choice-powered pathology made vivid.)

**Hausdorff measure**: $\mathcal{H}_s(A) = \lim_{\delta \to 0} \inf\big\{\sum_i d(U_i)^s : A \subset \bigcup_i U_i,\ d(U_i) \le \delta\big\}$. For each $A$ there's a critical $s_0$ with $\mathcal{H}_s(A) = \infty$ for $s < s_0$, $= 0$ for $s > s_0$: the **Hausdorff dimension** $\dim_{\mathcal{H}}(A) = \inf\{s : \mathcal{H}_s(A) = 0\}$. Scaling $\mathcal{H}_s(\alpha A) = \alpha^s \mathcal{H}_s(A)$; $\mathcal{H}_n \sim \lambda$ on $\mathbb{R}^n$. The tool for fractals.

## Measurable functions — the stability payoff

$f: \Omega \to \bar{\mathbb{R}}$ **measurable** $:\Leftrightarrow f^{-1}(B) \in \mathcal{F}$ for all Borel $B$ ⇔ $\{f > \alpha\} \in \mathcal{F}\ \forall \alpha \in \mathbb{R}$. Continuous ⇒ measurable. The property Riemann never had — measurability survives **everything**:
$$af + bg,\quad fg,\quad |f|,\quad \min/\max(f,g),\quad \sup_n f_n,\quad \inf_n f_n,\quad \limsup_n f_n,\quad \liminf_n f_n \;\text{ all measurable}$$
so **pointwise a.e. limits of measurable functions are measurable** (mechanism: $\{f + g > \alpha\} = \bigcup_{q \in \mathbb{Q}} \{f > q\} \cap \{g > \alpha - q\}$, $\{\sup_n f_n > \alpha\} = \bigcup_n \{f_n > \alpha\}$).

**The fundamental approximation**: $f \ge 0$ measurable ⇔ $f = \sup_n f_n$ for an *increasing* sequence of simple functions ($f = \sum_{i=1}^n \lambda_i 1_{A_i}$, finite range) — chop the *codomain* into dyadic layers $A_{j,n} = \{j 2^{-n} \le f < (j{+}1) 2^{-n}\}$. This is the Riemann/Lebesgue picture in one line: **Riemann partitions the domain, Lebesgue partitions the range** — wild domains are fine as long as level sets are measurable.

## The integral, in three stages

$$\text{1. simple: } \int \textstyle\sum_i \lambda_i 1_{A_i}\, d\mu = \sum_i \lambda_i \mu(A_i) \quad\to\quad \text{2. } f \ge 0: \int f = \lim_n \int f_n \ (f_n \uparrow f \text{ simple}) \quad\to\quad \text{3. } \int f = \int f^+ - \int f^-$$

with $f$ **integrable** $:\Leftrightarrow \int |f| \, d\mu < \infty$. Stage 2 is representation-independent; $\int f = 0 \Leftrightarrow f = 0$ a.e. (for $f \ge 0$).

**The three convergence theorems** — the whole reason for the construction:

| theorem | hypotheses | conclusion | mechanism |
|---|---|---|---|
| **Monotone convergence** | $0 \le f_n \uparrow f$ | $\int f_n \to \int f$ | continuity of measure |
| **Fatou** | $f_n \ge 0$ | $\int \liminf f_n \le \liminf \int f_n$ | MCT on $g_n = \inf_{k \ge n} f_k$; can be strict — mass escapes |
| **Dominated convergence** | $f_n \to f$ a.e., $\ \lvert f_n \rvert \le g$, $\int g < \infty$ | $\int \lvert f_n - f \rvert \to 0$ | Fatou applied to $2g - \lvert f_n - f \rvert$ |

Relation to Riemann: Riemann-integrable ⇒ Lebesgue-integrable, same value; bounded $f$ on $[a,b]$ Riemann-integrable **⇔ its discontinuity set is null**. Edge case: $\frac{\sin x}{x}$ on $[0,\infty)$ is improperly Riemann but not Lebesgue integrable ($\int |f| = \infty$). Bonus DCT application: $f$ differentiable with $|f'| \le M$ ⇒ $\int_a^b f' \, d\lambda = f(b) - f(a)$, no continuity of $f'$ needed. **Fubini** (stated, not proved): $f \ge 0$ or integrable w.r.t. $\mu_1 \otimes \mu_2$ ⇒ the double integral equals both iterated integrals.

## $L^p$ spaces — the payoff for functional analysis

$$L^p(\Omega) = \{f \text{ measurable} : \textstyle\int_\Omega |f|^p \, d\mu < \infty\} / \sim, \qquad \|f\|_p = \Big(\int_\Omega |f|^p \, d\mu\Big)^{1/p}, \qquad f \sim g :\Leftrightarrow f = g \text{ a.e.}$$

(quotienting is what makes $\|\cdot\|_p$ definite); $L^\infty$: $\|f\|_\infty = \operatorname{ess\,sup} |f| = \inf\{c : \mu(\{|f| \ge c\}) = 0\}$. Hölder $\|fg\|_1 \le \|f\|_p \|g\|_q$ and Minkowski transfer verbatim from [[Topological and Metric Spaces|the $\ell^p$ versions]].

- **Riesz–Fischer: $L^p$ complete**, $1 \le p \le \infty$ — reason (2) delivered; these are Banach spaces. *Mechanism:* fast Cauchy subsequence $\|f_{n_{k+1}} - f_{n_k}\|_p \le 2^{-k}$; dominate the telescoping series by $g \in L^p$ (MCT); DCT finishes. Byproduct worth remembering: $f_n \to f$ in $L^p$ ⇒ **some subsequence $f_{n_k} \to f$ a.e.** — the full sequence needn't (the "typewriter" sequence: $\to 0$ in $L^1[0,1]$, converges pointwise nowhere)
- $\mu(\Omega) < \infty$ ⇒ $L^p \subset L^q$ for $q \le p$ (bigger exponent = smaller space), dense inclusions. $L^p(\mathbb{R}^n)$ separable for $p < \infty$, with $C_c^\infty$ dense — the approximation backbone of PDE theory
- **Uniform convexity** ($1 < p < \infty$, via Clarkson): $\|f\|_p = \|g\|_p = 1,\ \|f - g\|_p \ge \varepsilon \Rightarrow \|\tfrac{f+g}{2}\|_p \le 1 - \delta(\varepsilon)$. Buys: reflexivity (**Milman–Pettis**: uniformly convex ⇒ reflexive) and the direct minimization proof of [[Dual Spaces and Weak Topologies|$(L^p)^* = L^q$]]. Fails for $p = 1, \infty$ — indeed the non-reflexive ones
- $\ell^p = L^p(\mathbb{N}, \text{counting measure})$

## Related

- [[Topological and Metric Spaces]] — completion, the inequality chain
- [[Dual Spaces and Weak Topologies]] — duals of $L^p$
- [[Hilbert Spaces]] — $L^2$, the case with geometry
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 3

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
