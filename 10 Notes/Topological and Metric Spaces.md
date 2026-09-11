---
type: concept
topics: [functional-analysis]
status: evergreen
created: 2026-09-10
aliases: [topology, metric space, Baire category theorem, completeness]
---

# Topological and Metric Spaces

> **Where this sits.** Chapter 1 of [[18.102 Functional Analysis]] — the substrate. Three layers: the *language* (topology: what "open", "convergent", "continuous" mean with no distances), the *working setting* (metric spaces: where sequences suffice), and the chapter's one deep theorem (**Baire**: completeness converted into a proof engine). Plus Zorn's lemma, the existence tool the whole course leans on.

## Zorn's lemma — the existence axiom

Poset, chain (totally ordered subset), upper bound, maximal element. **Zorn:** *if every chain has an upper bound, a maximal element exists.* Equivalent to the axiom of choice; treated as an axiom. Its job in this course: produce **highly non-unique objects** — every vector space has a Hamel basis (maximal linearly independent set); Hahn–Banach extensions; Tychonoff. Pattern of use: the desired object = a maximal element of "partial versions of the object, ordered by extension."

## Topological spaces — the minimal language

- **Topology** $\mathcal{T} \subset \mathcal{P}(X)$: contains $\emptyset, X$; closed under *arbitrary* unions and *finite* intersections. Closed = complement open. **Hausdorff**: distinct points have disjoint neighborhoods (limits unique)
- Interior = largest open set inside; closure $\overline{A}$ = smallest closed set containing; **dense**: $\overline{A} = X$; boundary = points whose every neighborhood meets both sides
- **Continuity** = preimages of open sets are open. Continuous ⇒ sequentially continuous; converse needs **first countability** (countable neighborhood base at each point) — the reason sequences don't suffice in general topologies but do in metric spaces
- **Initial topology**: the *coarsest* topology making a given family of maps continuous. File this one — it is *the* construction behind the [[Dual Spaces and Weak Topologies|weak and weak-* topologies]] of ch. 4; the product topology is the special case induced by coordinate projections
- Countability ladder: second countable (countable base) ⇒ first countable + separable (countable dense set)
- **Compactness**: every open cover has a finite subcover; equivalently every family of closed sets with the finite intersection property has common point. **Tychonoff**: arbitrary products of compacts are compact (proved with Zorn; the muscle behind [[Dual Spaces and Weak Topologies|Banach–Alaoglu]])
- **Topological vector space**: Hausdorff topology making $+$ and scalar multiplication continuous. Warning example kept for motivation: $L^p$, $0<p<1$, is a TVS whose *continuous dual is trivial* — the disease [[Normed and Banach Spaces|Hahn–Banach]] rules out for normed spaces

## Metric spaces — where sequences suffice

- **Metric**: definiteness, symmetry, triangle inequality. The induced topology is Hausdorff and first countable ⇒ **everything is testable on sequences** (closedness, continuity, compactness)
- **Complete**: every Cauchy sequence converges. Two standing warnings: completeness is a *metric* property, not topological (same topology can come from a complete and an incomplete metric); closed ⊂ complete-space ⇔ complete
- **Completion theorem**: every metric space embeds isometrically as a dense subset of a complete one, uniquely up to isometry. Construction: equivalence classes of Cauchy sequences. (This is one of *two* routes to $L^p$ — the abstract one; ch. 3's measure theory is the concrete one)
- Metric specials: compact ⇔ sequentially compact; separable ⇔ second countable

**The inequality chain** (proved here, used everywhere): concavity of log → **Young** $ab \le \frac{a^p}{p} + \frac{b^q}{q}$ → **Hölder** $\sum|x_n y_n| \le \|x\|_p \|y\|_q$ (conjugate exponents $\tfrac1p + \tfrac1q = 1$; the conjugacy is *necessary*, not aesthetic) → **Minkowski** = the triangle inequality for $\|\cdot\|_p$.

**The example zoo** (the course's recurring test cases):

| space | complete? | separable? |
|---|---|---|
| $\ell^p$, $1 \le p < \infty$ | ✅ | ✅ |
| $\ell^\infty$ (sup norm) | ✅ | ❌ — the standard "too big" space |
| $c_0$ (null sequences) | ✅ | ✅ |
| $c_c$ (finitely supported) | ❌ — dense in $c_0$ | ✅ |
| $C(K)$, $K$ compact, sup metric | ✅ (uniform limit of continuous is continuous) | ✅ |

## Baire category theorem — completeness as an engine

Vocabulary: **nowhere dense** (closure has empty interior) → **meager** = countable union of nowhere dense sets ("small") → **residual / Baire-generic** = complement of meager ("almost all").

**Baire:** *in a complete metric space, a countable intersection of open dense sets is dense.* Equivalently: **a complete metric space is never a countable union of nowhere dense sets.** Proof mechanism: nest shrinking closed balls, one per open dense set; completeness delivers the limit point.

Two reasons it's here:

1. **It powers ch. 2's three cornerstones** ([[Normed and Banach Spaces|uniform boundedness, open mapping, closed graph]]) — each is Baire applied to a cleverly chosen countable decomposition. This is the course's main use
2. **Genericity arguments**: the Baire-generic continuous function on $[0,1]$ is *nowhere differentiable* (Weierstraß's "monster" is the typical case, not the exception) — the template for "most objects are wild" proofs

## Related

- [[Normed and Banach Spaces]] — the next layer of structure; Baire's payoff
- [[Dual Spaces and Weak Topologies]] — initial topologies and Tychonoff, cashed in
- [[Functional Analysis Definitions]] — the glossary

## Sources

- [[Kehle - 18.102 Functional Analysis Lecture Notes (2025)]] — ch. 1

---
Part of the 18.102 cluster — map: [[18.102 Functional Analysis]].
