---
type: concept
topics: [swe-interview]
status: evergreen
created: 2026-09-13
aliases: [interview probability, brainteasers, expected value questions]
---

# Probability and Brainteasers

> **Where this sits.** [[SWE Interview Prep]] — Citadel runs probability into SWE loops (expected-value puzzles, multi-stage decisions, Bayes). You have the measure-theoretic version already; this note is the *interview dialect*: five techniques + the classics table, drilled to instant recall. The grading is on **setup speed** — naming the right technique in the first 30 seconds.

## The five techniques

**1. Linearity of expectation + indicator variables.** $\mathbb{E}[\sum X_i] = \sum \mathbb{E}[X_i]$, **independence not required** — that clause is the whole trick. Recipe: decompose the quantity into 0/1 indicators, sum their probabilities.
- Expected fixed points of a random permutation: $\sum_{i=1}^n \mathbb{P}(\pi(i) = i) = n \cdot \tfrac1n = 1$
- Expected # of head-runs in $n$ flips: indicators on "run starts at $i$"
- **Coupon collector**: time to see all $n$ types $= n \sum_{k=1}^{n} \tfrac1k \approx n \ln n$ (waiting times $\tfrac{n}{n-k+1}$, summed)

**2. First-step analysis (condition on the first move).** Set up $\mathbb{E}[\cdot]$ or $\mathbb{P}(\cdot)$ as unknowns, condition on step one, solve the linear system.
- Expected flips to first H: $E = 1 + \tfrac12 E \Rightarrow E = 2$
- Expected flips to **HH**: $E = 6$; to **HT**: $E = 4$. Why they differ — the answer to *say*: after progress "H", a tail resets HH to zero but is itself progress toward HT; HT's progress is never destroyed
- **Gambler's ruin** (fair): start $a$, absorb at $0$ or $n$ — $\mathbb{P}(\text{reach } n) = a/n$; expected duration $a(n - a)$. Biased $p \neq \tfrac12$: ratios of $((1-p)/p)^k$
- Markov-chain phrasing is the same machinery with more states — draw the chain, write one equation per state

**3. Symmetry and exchangeability.** Before computing, ask what's invariant.
- Random walk, ballot-style problems: reflection principle
- "$k$-th card is an ace" has probability $4/52$ for *every* $k$ — positions are exchangeable
- $n$ points on a circle all in one semicircle: $n / 2^{n-1}$ (condition on the "leading" point)

**4. Bayes / conditioning.** $\mathbb{P}(A \mid B) = \mathbb{P}(B \mid A)\mathbb{P}(A) / \mathbb{P}(B)$; expand $\mathbb{P}(B)$ by total probability. The base-rate classic: disease prevalence 1/1000, test 99% accurate both ways → $\mathbb{P}(\text{sick} \mid +) = \frac{.99 \cdot .001}{.99 \cdot .001 + .01 \cdot .999} \approx 9\%$ — the posterior is dominated by the false positives because the base rate is tiny. Also here: Monty Hall (switch wins $2/3$ — the host's reveal is *informative* because he never opens the car door); two-children puzzles (the answer depends on *how* you learned "at least one boy" — state the sampling mechanism).

**5. Compute the complement / expectation of the max.** $\mathbb{P}(\text{at least one}) = 1 - \mathbb{P}(\text{none})$; for maxima use $\mathbb{P}(\max \le k) = \mathbb{P}(\text{all} \le k)$, then $\mathbb{E}[\max] = \sum_{k \ge 1} \mathbb{P}(\max \ge k)$.
- Birthday: $\mathbb{P}(\text{collision among } m) = 1 - \prod_{k=0}^{m-1}(1 - \tfrac{k}{365})$; crosses $\tfrac12$ at $m = 23$; rule of thumb: collisions appear at $m \sim \sqrt{n}$ — also the hash-collision heuristic
- Expected max of two dice: $\mathbb{E}[\max] = \sum_k \mathbb{P}(\max \ge k) = 161/36 \approx 4.47$

## The algorithmic-randomness crossover (SWE-specific)

These are coding questions wearing probability clothes — know both the algorithm and its proof:

- **Fisher–Yates shuffle**: `for i in n-1..1: swap(a[i], a[rand(0..i)])` — uniform over all $n!$ permutations (induction). The buggy variant `rand(0..n-1)` every step is *not* uniform — classic spot-the-bug
- **Reservoir sampling** (uniform sample of size 1 from a stream of unknown length): keep item $i$ with probability $1/i$. Proof: survival telescopes, $\tfrac1i \cdot \prod_{j > i} \tfrac{j-1}{j} = \tfrac1n$
- **Rejection sampling**: rand7 from rand5 — draw a $5\times5$ grid value, keep if $\le 21$, mod 7; expected draws = geometric
- **Randomized quickselect**: expected $O(n)$ — linearity over comparison indicators

## The classics card (drill to instant)

| question | answer | technique |
|---|---|---|
| E[flips to HH] / to HT | 6 / 4 | first-step |
| E[rolls to see all 6 faces] | $6 H_6 = 14.7$ | coupon collector |
| P(reach $n$ before 0 from $a$), fair | $a/n$ | gambler's ruin |
| E[fixed points of random permutation] | 1 | indicators |
| birthday collision ½ | 23 people | complement |
| P(sick \| +), 1/1000 prev., 99% test | ≈ 9% | Bayes |
| Monty Hall switch | 2/3 | conditioning |
| E[max of 2 dice] | 161/36 | tail sum |
| stream sample | keep $i$ w.p. $1/i$ | reservoir |

## How to perform it

State the technique first ("indicators + linearity"), write the decomposition, *then* compute — partial credit lives in the setup. If stuck 30 seconds, say the honest thing: "let me condition on the first step" — it solves more of these than any other single move. Sanity-check every answer against an extreme case ($p \to 0$, $n = 1$, symmetry).

## Related

- [[Algorithmic Patterns for Interviews]] — the shuffle/sampling overlap
- [[SWE Interview Prep]] — where this sits in the pipeline

---
Part of the interview-prep cluster — map: [[SWE Interview Prep]].
