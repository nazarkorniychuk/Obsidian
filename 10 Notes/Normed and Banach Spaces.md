---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [Banach space, bounded operator, Hahn-Banach, open mapping theorem, closed graph theorem, uniform boundedness]
---

# Normed and Banach Spaces

> **Where this sits.** Chapter 2 of [[18.102 Functional Analysis]] — the home objects. A norm welds [[Topological and Metric Spaces|metric structure]] onto a vector space; **Banach = complete normed**. Three acts: what infinite dimensionality costs (no compact unit ball, discontinuous linear maps), Hahn–Banach (duals are rich), and the three Baire-powered **cornerstones**. Every entry: *Idea* → *Formally* → *Mechanism*.

## The objects, and the dividing line

**Norm, Banach space.** *Idea:* a length function compatible with the linear structure — scaling scales it, the triangle inequality holds, and only $0$ has length $0$; Banach means additionally no missing points. *Formally:* $\|\alpha v\| = |\alpha| \|v\|$; $\|v + w\| \le \|v\| + \|w\|$; $\|v\| = 0 \Rightarrow v = 0$ (drop the last: **seminorm**); metric $d(v,w) = \|v - w\|$; Banach $:=$ complete. Every normed space has a completion (Banach, containing it isometrically as a dense subspace, unique).

**Schauder basis.** *Idea:* a basis adapted to analysis — every vector is a convergent *series*, not a finite sum, with unique coefficients. *Formally:* $\{e_n\}$ with $\forall x\ \exists! (x_n) \subset \mathbb{K}$: $\|x - \sum_{n=1}^{N} x_n e_n\| \to 0$. Schauder basis ⇒ separable; converse false.

**Riesz's lemma.** *Idea:* even without an inner product you can find a unit vector "almost perpendicular" to any proper closed subspace — almost all of its length sticks out. *Formally:* $Y \subsetneq X$ closed, $\varepsilon > 0$ ⇒ $\exists x, \|x\| = 1, \operatorname{dist}(x, Y) \ge 1 - \varepsilon$. *Mechanism:* take $z \notin Y$, pick $y \in Y$ nearly achieving $d = \operatorname{dist}(z, Y) > 0$, normalize $z - y$.

**The dividing line of the subject.** *Idea:* the unit ball is compact exactly in finite dimensions — infinite-dimensional analysis is the art of living without that compactness. *Formally:*
$$\overline{B}_1(0) \text{ compact} \iff \dim X < \infty$$
*Mechanism:* (⇐) equivalence of norms + Heine–Borel; (⇒) Riesz's lemma iterates unit vectors with mutual distance $\ge \tfrac12$ — a sequence with no convergent subsequence. [[Dual Spaces and Weak Topologies|Ch. 4]] is the project of buying compactness back with coarser topologies.

## Linear operators: bounded = continuous

**Bounded operator.** *Idea:* a linear map that magnifies lengths by at most a fixed factor; for linear maps this modest condition *is* continuity, because linearity spreads control at one point everywhere. *Formally:* $\|T\| = \sup_{x \neq 0} \frac{\|Tx\|_W}{\|x\|_V} < \infty$; bounded ⇔ continuous ⇔ continuous at a single point. If $\dim V < \infty$, every linear map is bounded — discontinuous linear maps are a purely infinite-dimensional phenomenon.

**The founding example pair.** *Idea:* on continuous functions, integrating is tame, differentiating is violent — and the violent one is the one analysis cares about, which is why the theory drags domains $\operatorname{dom}(T) \subsetneq X$ along forever. *Formally:* on $(C[0,1], \|\cdot\|_\infty)$: $T_I f = \int_0^x f$ has $\|T_I\| = 1$; $T_D f = f'$ on $\operatorname{dom} = C^1$ is unbounded — $f_n = \sin(nx)$ gives $\|f_n\|_\infty \le 1$, $\|T_D f_n\|_\infty \ge n$.

**The space of operators.** *Idea:* operators themselves form a normed space, and it inherits completeness from the *target* — so dual spaces are always complete, no matter how defective the source. *Formally:* $BL(X,Y)$ with the operator norm is Banach whenever $Y$ is; in particular $X^* = BL(X, \mathbb{K})$ is always Banach.

**Bounded linear extension.** *Idea:* a bounded operator defined on a dense subset extends uniquely to everything, keeping its norm — the standard way operators on $L^p$ get defined (declare on nice functions, extend). *Formally:* $T$ bounded on $\operatorname{dom}(T)$ dense in $X$, $Y$ Banach ⇒ $\exists!\ \tilde{T} \in BL(X, Y)$ with $\tilde{T}|_{\operatorname{dom}(T)} = T$, $\|\tilde{T}\| = \|T\|$. *Mechanism:* $\tilde{T}x = \lim T x_n$ along any $x_n \to x$; boundedness makes the limit exist and not depend on the sequence.

## Hahn–Banach: duals are rich

**Hahn–Banach extension theorem.** *Idea:* any linear functional defined on a subspace and fenced in by a convexity gauge can be extended to the *whole space without ever breaking the fence* — functionals never get stuck. *Formally (real):* $p: V \to \mathbb{R}$ sublinear ($p(x+y) \le p(x) + p(y)$, $p(\lambda x) = \lambda p(x)$ for $\lambda \ge 0$), $f: W \to \mathbb{R}$ linear on a subspace with $f \le p$ on $W$ ⇒ $\exists$ linear $f^*: V \to \mathbb{R}$, $f^*|_W = f$, $f^* \le p$ on $V$. (Complex: $|f| \le p$ for a seminorm $p$.) *Mechanism:* extend one dimension at a time — the two-sided constraint $\sup_y (f(y) - p(y - v)) \le f^*(v) \le \inf_x (p(x + v) - f(x))$ is satisfiable — then Zorn to exhaust.

**The corollaries that get used.** *Idea:* the dual is never poor: it sees every vector's norm, distinguishes any two vectors, and detects any point outside a closed subspace. *Formally:*

- $\forall v \neq 0\ \exists f \in X^*: \|f\| = 1,\ f(v) = \|v\|$, hence $\displaystyle \|x\| = \sup_{\|f\| = 1} |f(x)|$ (contrast: TVS like $L^p$, $p < 1$, where $X^* = \{0\}$)
- $U \subsetneq X$ closed, $x \notin U$ ⇒ $\exists f \in X^*: f|_U = 0, f(x) \neq 0$ (*mechanism:* norming functional in the quotient $X/U$, pulled back)

**Bidual and reflexivity.** *Idea:* every space sits isometrically inside its double dual via "evaluate at me"; reflexive means this copy is everything — the space already knows its own second-order structure. *Formally:* $i: X \to X^{**}$, $i(x)(f) = f(x)$, always isometric (by the sup formula); reflexive $:\Leftrightarrow i$ surjective. Scoreboard: finite-dim, $\ell^p, L^p$ ($1 < p < \infty$) reflexive; $(c_0)^* \cong \ell^1$, $(\ell^1)^* \cong \ell^\infty$, none of $c_0, \ell^1, \ell^\infty$ reflexive. Also: $X^*$ separable ⇒ $X$ separable (converse false: $\ell^1$ vs $\ell^\infty$).

## The three cornerstones — Baire cashed in

*Idea for all three at once:* completeness is a hypothesis you can spend. Via [[Topological and Metric Spaces#Baire category theorem — completeness as an engine|Baire]], a Banach space is not a countable union of thin sets — so in any countable decomposition $X = \bigcup_m A_m$ some $A_m$ is somewhere dense, and linearity spreads that one dense patch to the whole space. Each cornerstone is this move with a different decomposition.

**1. Uniform boundedness (Banach–Steinhaus).** *Idea:* if a family of operators is bounded at every single point, it is bounded uniformly — pointwise control upgrades to global control for free. *Formally:* $X$ Banach, $(T_n) \subset BL(X,Y)$:
$$\sup_n \|T_n x\| < \infty \ \ \forall x \in X \quad\Longrightarrow\quad \sup_n \|T_n\| < \infty$$
*Mechanism:* Baire on $A_m = \{x : \sup_n \|T_n x\| \le m\}$; a ball inside some $A_M$ + linearity rescales to the unit ball.

**2. Open mapping theorem.** *Idea:* a bounded surjection between Banach spaces cannot compress its image topologically — it maps open sets to open sets; surjectivity plus completeness forces openness. *Formally:* $X, Y$ Banach, $T \in BL(X,Y)$ surjective ⇒ $T$ open, i.e. $\exists \varepsilon: B_\varepsilon^Y(0) \subset T(B_1^X(0))$. *Mechanism:* Baire on $Y = \bigcup_n \overline{T(B_n(0))}$ gives an interior point of a closure; translation + scaling puts a ball inside $\overline{T(B_1(0))}$; an iterative approximation (geometric series of preimages, completeness of $X$) removes the closure. Needs completeness of **both** spaces.

- **Inverse mapping theorem.** *Idea:* for bounded bijections between Banach spaces, the inverse is automatically continuous — algebraic invertibility is topological invertibility. *Formally:* $T \in BL(X,Y)$ bijective ⇒ $T^{-1} \in BL(Y,X)$
- **Closed range criterion.** *Formally:* $T$ injective bounded: $T^{-1}$ continuous on $\operatorname{range}(T)$ ⇔ $\operatorname{range}(T)$ closed

**3. Closed graph theorem.** *Idea:* to prove an everywhere-defined operator on a Banach space is continuous, you may *assume the conclusion halfway*: take $x_n \to x$, **assume** $T x_n$ converges, and only verify the limit is $Tx$. Completeness pays for the assumed half. *Formally:* $\operatorname{dom}(T) = X$ Banach, $\operatorname{graph}(T) = \{(x, Tx)\}$ closed in $X \times Y$ ⇒ $T$ bounded. Contrast made precise: continuity = "$x_n \to x$ ⇒ $Tx_n$ converges *and* to $Tx$"; closedness = "$x_n \to x$ *and* $Tx_n \to y$ ⇒ $y = Tx$." *Mechanism:* $\operatorname{graph}(T)$ is itself Banach; the projection to $X$ is a bounded bijection; inverse mapping theorem. Separating example: $T_D$ on $\operatorname{dom} = C^1 \subset C[0,1]$ is **closed but unbounded** — no contradiction, $C^1$ is not closed in $C[0,1]$.

## Related

- [[Topological and Metric Spaces]] — Baire, the engine under all three cornerstones
- [[Dual Spaces and Weak Topologies]] — what the rich dual is *for*
- [[Hilbert Spaces]] — the special case where the dual is the space itself
- [[Functional Analysis Definitions]] — glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 2

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
