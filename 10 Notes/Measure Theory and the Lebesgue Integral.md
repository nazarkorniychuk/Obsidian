---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [measure theory, Lebesgue integral, Lp spaces, dominated convergence, Riesz-Fischer, Caratheodory]
---

# Measure Theory and the Lebesgue Integral

> **Where this sits.** Chapter 3 of [[18.102 Functional Analysis]] — the longest chapter, and the construction of the course's main *examples*: the $L^p$ spaces. Three reasons Riemann isn't enough, each naming a payoff below: **(1)** integrate over general sets, not just intervals ($1_{\mathbb{Q}}$ becomes integrable); **(2)** completeness — Riemann-integrable functions under $\int|f-g|$ are *not* complete, and $L^1$ is exactly the missing completion; **(3)** convergence theorems — Riemann tolerates only uniform limits, Lebesgue tolerates pointwise-a.e. + domination. (Also: the foundation of modern probability.) Every entry: *Idea* → *Formally* → *Mechanism*.

## Measure spaces — the axioms of "volume"

**σ-algebra and measure.** *Idea:* first decide *which* sets get a volume (a family stable under the set operations countable processes produce), then assign volumes so that cutting into countably many disjoint pieces never creates or destroys mass. *Formally:* $\mathcal{F} \subset \mathcal{P}(\Omega)$ with $\emptyset \in \mathcal{F}$; $A \in \mathcal{F} \Rightarrow A^c \in \mathcal{F}$; $A_i \in \mathcal{F} \Rightarrow \bigcup_{i \in \mathbb{N}} A_i \in \mathcal{F}$. Measure: $\mu: \mathcal{F} \to [0,\infty]$, $\mu(\emptyset) = 0$, $\mu(\bigsqcup_i A_i) = \sum_i \mu(A_i)$. **Null set**: $\mu(A) = 0$; "**a.e.**" = off a null set — the resolution at which all of $L^p$ theory operates.

**Continuity of measure.** *Idea:* volumes pass to limits along increasing or (with one finite) decreasing chains of sets — countable additivity's workhorse form. *Formally:* $E_n \uparrow \Rightarrow \mu(\bigcup_n E_n) = \lim_n \mu(E_n)$; $E_n \downarrow$, some $\mu(E_n) < \infty \Rightarrow \mu(\bigcap_n E_n) = \lim_n \mu(E_n)$. *Mechanism:* disjointify $\tilde{E}_n = E_n \setminus E_{n-1}$ and add. **Borel σ-algebra**: $\mathcal{B} = \sigma(\mathcal{T})$, the smallest σ-algebra containing the open sets.

## Carathéodory's machine — why not all sets are measurable

**Vitali's impossibility.** *Idea:* you cannot have everything — no notion of length can simultaneously measure *all* subsets of $\mathbb{R}$, give intervals their length, be translation invariant, and be countably additive. The right sacrifice is universality. *Formally:* no $\mu: \mathcal{P}(\mathbb{R}) \to [0,\infty]$ satisfies all four; the witness set is built with the axiom of choice.

**Outer measure → Carathéodory criterion → extension theorem.** *Idea:* define a rough over-estimate on *all* sets (cover and sum); then call a set measurable if it *slices every other set cleanly* — no test set loses or gains mass when cut along $A$; on those sets the estimate is a genuine measure. *Formally:* outer measure: $\mu^*(\emptyset) = 0$, monotone, $\mu^*(\bigcup_i A_i) \le \sum_i \mu^*(A_i)$. Criterion:
$$A \text{ measurable} \;:\Longleftrightarrow\; \mu^*(B) = \mu^*(B \cap A) + \mu^*(B \cap A^c) \quad \forall B \subset \Omega$$
Extension theorem: the measurable sets form a σ-algebra, $\mu = \mu^*|_{\mathcal{F}}$ is a *complete* measure (subsets of null sets are measurable and null). *Mechanism:* the criterion is symmetric in $A, A^c$ and survives countable disjoint unions by an induction + limit argument; subadditivity supplies all the "$\le$" halves for free.

**Lebesgue measure.** *Idea:* the outer estimate by open-box covers, fed through the machine — the unique translation-invariant volume extending "length × width × …". *Formally:* $\lambda^*(A) = \inf\{\sum_i \operatorname{vol}(C_i) : C_i$ open cuboids, $A \subset \bigcup_i C_i\}$; $\lambda = \lambda^*|_{\mathcal{L}}$. Facts: $\lambda(C) = \operatorname{vol}(C)$; $\lambda(A + x) = \lambda(A)$; open/closed sets measurable; **regular**: $\lambda(A) = \sup_{K \subset A \, \text{cpt}} \lambda(K) = \inf_{O \supset A \, \text{open}} \lambda(O)$; $\mathcal{L}$ = completion of Borel ($A \in \mathcal{L} \Leftrightarrow$ Borel ∪ null). Non-measurable sets exist *only via choice* — rule of thumb: any set you can write down is measurable. (Banach–Tarski: the choice-powered pathology made vivid.)

**Hausdorff measure and dimension.** *Idea:* measure with the exponent $s$ as a dial — cover by small sets, weight each by (diameter)$^s$; for every set there is one critical exponent where the answer jumps from $\infty$ to $0$, and *that number is the set's dimension*. *Formally:*
$$\mathcal{H}_s(A) = \lim_{\delta \to 0}\, \inf\Big\{\sum_i d(U_i)^s : A \subset \bigcup_i U_i,\ d(U_i) \le \delta\Big\}, \qquad \dim_{\mathcal{H}}(A) = \inf\{s : \mathcal{H}_s(A) = 0\}$$
Scaling $\mathcal{H}_s(\alpha A) = \alpha^s \mathcal{H}_s(A)$; $\mathcal{H}_n \sim \lambda$ on $\mathbb{R}^n$. The tool for fractals.

## Measurable functions — the stability payoff

**Measurable function.** *Idea:* a function whose level sets are measurable — the minimal requirement for "area under the graph" to make sense when you slice along the *range*. *Formally:* $f: \Omega \to \bar{\mathbb{R}}$ with $f^{-1}(B) \in \mathcal{F}$ for all Borel $B$ ⇔ $\{f > \alpha\} \in \mathcal{F}\ \forall \alpha \in \mathbb{R}$. Continuous ⇒ measurable.

**Stability under everything.** *Idea:* the property Riemann never had — measurability survives algebra *and limits*, so the class is closed under every operation analysis performs. *Formally:*
$$af + bg,\ fg,\ |f|,\ \min/\max(f,g),\ \sup_n f_n,\ \inf_n f_n,\ \limsup_n f_n,\ \liminf_n f_n \text{ all measurable}$$
hence pointwise a.e. limits of measurable functions are measurable. *Mechanism:* $\{f + g > \alpha\} = \bigcup_{q \in \mathbb{Q}} \{f > q\} \cap \{g > \alpha - q\}$; $\{\sup_n f_n > \alpha\} = \bigcup_n \{f_n > \alpha\}$.

**Monotone approximation by simple functions.** *Idea:* every non-negative measurable function is an increasing limit of "finitely many horizontal slabs" — chop the *codomain* dyadically. This is the Riemann/Lebesgue contrast in one line: **Riemann partitions the domain, Lebesgue partitions the range**, so wild domains are fine as long as level sets are measurable. *Formally:* $f \ge 0$ measurable ⇔ $\exists$ simple $0 \le f_1 \le f_2 \le \cdots$ with $\sup_n f_n = f$; construction $A_{j,n} = \{j 2^{-n} \le f < (j+1) 2^{-n}\}$, $f_n = \sum_j \tfrac{j}{2^n} \mathbf{1}_{A_{j,n}}$.

## The integral, in three stages

*Idea:* define the integral where it's obvious (slabs), extend by monotone limits (well-defined by a comparison lemma), then split signed functions into positive and negative parts. *Formally:*

$$\text{1. } \int \textstyle\sum_i \lambda_i \mathbf{1}_{A_i}\, d\mu = \sum_i \lambda_i \mu(A_i) \;\;\to\;\; \text{2. } f \ge 0: \int f = \lim_n \int f_n\ (f_n \uparrow f \text{ simple}) \;\;\to\;\; \text{3. } \int f = \int f^+ - \int f^-$$

$f$ **integrable** $:\Leftrightarrow \int |f| \, d\mu < \infty$. For $f \ge 0$: $\int f = 0 \Leftrightarrow f = 0$ a.e.

**The three convergence theorems.** *Idea:* the whole reason for the construction — conditions under which $\lim \int = \int \lim$. Monotone: increasing limits always pass. Fatou: for non-negative functions the integral can only *lose* mass in the limit, never gain. Dominated: a single integrable ceiling tames everything. *Formally:*

| theorem | hypotheses | conclusion | mechanism |
|---|---|---|---|
| **Monotone convergence** | $0 \le f_n \uparrow f$ | $\int f_n \to \int f$ | continuity of measure on level sets |
| **Fatou** | $f_n \ge 0$ | $\int \liminf f_n \le \liminf \int f_n$ | MCT on $g_n = \inf_{k \ge n} f_k$; strictness = escaping mass |
| **Dominated convergence** | $f_n \to f$ a.e., $\lvert f_n \rvert \le g$, $\int g < \infty$ | $\int \lvert f_n - f \rvert \to 0$ | Fatou applied to $2g - \lvert f_n - f \rvert$ |

**Riemann, situated.** *Formally:* Riemann-integrable ⇒ Lebesgue-integrable with the same value; bounded $f$ on $[a,b]$ is Riemann-integrable ⇔ its discontinuity set is null. Edge case: $\frac{\sin x}{x}$ on $[0, \infty)$ — improperly Riemann integrable, not Lebesgue ($\int |f| = \infty$). DCT bonus: $f$ differentiable with $|f'| \le M$ ⇒ $\int_a^b f' \, d\lambda = f(b) - f(a)$, no continuity of $f'$ needed. **Fubini** (stated): for $f \ge 0$ or integrable w.r.t. $\mu_1 \otimes \mu_2$, the double integral equals both iterated integrals.

## $L^p$ spaces — the payoff for functional analysis

**The spaces.** *Idea:* functions with finite $p$-th-power "energy," where two functions equal a.e. count as the same — that identification is what turns the seminorm into a norm. *Formally:*
$$L^p(\Omega) = \{f \text{ msble}: \textstyle\int_\Omega |f|^p d\mu < \infty\} / \sim, \quad \|f\|_p = \Big(\int_\Omega |f|^p \, d\mu\Big)^{1/p}, \quad f \sim g :\Leftrightarrow f = g \text{ a.e.}$$
$L^\infty$: $\|f\|_\infty = \operatorname{ess\,sup}|f| = \inf\{c : \mu(\{|f| \ge c\}) = 0\}$. Hölder $\|fg\|_1 \le \|f\|_p \|g\|_q$ and Minkowski transfer verbatim from [[Topological and Metric Spaces|the $\ell^p$ versions]]; $\ell^p = L^p(\mathbb{N}, \text{counting})$.

**Riesz–Fischer.** *Idea:* $L^p$ is complete — the promised completion of the Riemann world; these are Banach spaces, and all of functional analysis applies to them. *Formally:* $L^p(\Omega)$ complete for $1 \le p \le \infty$. *Mechanism:* pick a fast Cauchy subsequence ($\|f_{n_{k+1}} - f_{n_k}\|_p \le 2^{-k}$), dominate the telescoping series by a $g \in L^p$ (MCT), conclude a.e. convergence, finish with DCT. Byproduct: **$L^p$-convergence ⇒ some subsequence converges a.e.** — the full sequence needn't (the "typewriter" sequence: $\to 0$ in $L^1[0,1]$, converges pointwise nowhere).

**Structure facts.** *Formally:* $\mu(\Omega) < \infty$ ⇒ $L^p \subset L^q$ for $q \le p$, dense inclusions (bigger exponent = smaller space). $L^p(\mathbb{R}^n)$ separable for $p < \infty$, with $C_c^\infty$ dense — the approximation backbone of PDE theory.

**Uniform convexity.** *Idea:* in $L^p$ ($1 < p < \infty$) the unit ball is "round": midpoints of distant unit vectors sit strictly inside. Roundness is quantitative rigidity — it forces minimizing sequences to be Cauchy, which is exactly how [[Dual Spaces and Weak Topologies|$(L^p)^* = L^q$]] gets proved, and it implies reflexivity (**Milman–Pettis**). *Formally:* $\forall \varepsilon\ \exists \delta$: $\|f\|_p = \|g\|_p = 1, \|f - g\|_p \ge \varepsilon \Rightarrow \|\tfrac{f+g}{2}\|_p \le 1 - \delta$. *Mechanism:* Clarkson's inequalities (pointwise convexity estimates integrated up). Fails for $p = 1, \infty$ — indeed the non-reflexive ones.

## Related

- [[Topological and Metric Spaces]] — completion, the inequality chain
- [[Dual Spaces and Weak Topologies]] — duals of $L^p$
- [[Hilbert Spaces]] — $L^2$, the case with geometry
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 3

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
