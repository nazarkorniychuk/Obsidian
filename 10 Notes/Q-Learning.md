---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [Q-learning, Watkins Q-learning, tabular Q-learning, overestimation bias]
---

# Q-Learning

> **Where this sits.** The [[Reinforcement Learning|master loop]] is ACT → EVALUATE → IMPROVE. Q-learning is the most radical compression of it: **IMPROVE is folded into EVALUATE's target** — every single update simultaneously estimates values *and* pushes them toward optimal ones. It's [[Bellman Equation|value iteration]] run from samples, and the head of the value-based branch.

## The idea: aim every update directly at Q*

[[Temporal Difference Learning|TD]] learns whatever its target points at. Point the target at the [[Bellman Equation|optimality equation]] — put a $\max$ inside — and you skip the evaluate-then-improve loop entirely:

$$Q(s,a) \;\leftarrow\; Q(s,a) + \alpha\big[\underbrace{r + \gamma \max_{a'} Q(s',a')}_{\text{target: sampled optimality backup}} - \; Q(s,a)\big]$$

Term by term: you experienced one transition $(s, a, r, s')$; the target says *"the reward I actually got, plus γ times the best I currently believe I could do from where I landed"*; the difference from the current entry is the TD error; α damps the noise. Numeric micro-run: $Q(s,a) = 1$, you observe $r = 2$, land in $s'$ where the best entry is $4$, with $\gamma = 0.5$, $\alpha = 0.1$: target $= 2 + 0.5 \cdot 4 = 4$, error $= +3$, new $Q(s,a) = 1.3$.

Two structural facts, both inherited from the foundations:

- **It's Q, not V, for a reason:** acting from the learned table is a model-free lookup, $\arg\max_a Q(s,a)$ — the [[Bellman Equation|Q-vs-V argument]]
- **It's off-policy by construction:** the $\max$ makes the target refer to the *greedy* policy regardless of how you actually behaved (behavior policy ≠ target policy — the [[Reinforcement Learning|two-policies split]]). You can explore however you like, follow another agent, or replay old data — the estimates still aim at $Q^*$

## What's guaranteed — and what the guarantee demands

**Result** ([[Q-learning - Watkins & Dayan (1992)|Watkins & Dayan 1992]]): tabular Q-learning converges to $Q^*$ with probability 1, provided

1. **every state–action pair is visited infinitely often** — the coverage precondition; the algorithm supplies zero exploration of its own ([[Exploration vs Exploitation]] is how you satisfy this), and
2. step sizes satisfy Robbins–Monro: $\sum \alpha = \infty$ (can travel anywhere), $\sum \alpha^2 < \infty$ (noise averages out)

Read the fine print: *tabular*. One cell per $(s,a)$, updated exactly. The theorem says nothing about function approximation — deliberately, as it turns out (below).

## The built-in flaw: the max overestimates

The update uses the **maximum of estimates** as a stand-in for the **maximum of true values** — and that substitution is positively biased whenever estimates are noisy ([[Double Q-learning (2010)|van Hasselt 2010]]).

Why, concretely: suppose 10 actions all have *true* value 0, but each estimate carries independent noise of ±1. The max over the 10 estimates picks whichever got lucky — its expected value is ≈ +1.5, not 0. **The max doesn't select the best action; it selects the best-looking error.** Formally $\mathbb{E}[\max_a \hat{Q}(a)] \ge \max_a \mathbb{E}[\hat{Q}(a)]$, with equality only when there's no noise. Two aggravating factors:

- **It compounds:** the inflated max goes *into the bootstrap target*, so the overestimate is copied into upstream states, which feed further targets — bias propagates through the [[Bellman Equation|Bellman recursion]] instead of averaging away
- **It's uneven:** noise is largest exactly where data is scarce, so the bias systematically inflates poorly-explored regions — the opposite of the caution you'd want there

**Result:** in stochastic environments this makes plain Q-learning perform *very poorly* — not a corner case ([[Double Q-learning (2010)|van Hasselt 2010]]).

## The fix: decouple selection from evaluation

**Double Q-learning**: maintain two independent tables $Q^A, Q^B$. To build a target, use one to *choose* the best action and the other to *score* it:

$$\text{target} = r + \gamma\, Q^B\big(s', \arg\max_{a'} Q^A(s', a')\big)$$

If $Q^A$'s argmax picked a lucky error, $Q^B$ — with *independent* noise — scores it honestly; the luck no longer pays. **Results:** the double estimator removes the overestimation (at the price of possible mild *under*estimation), provably converges to the optimal policy, and dramatically outperforms Q-learning where rewards are noisy ([[Double Q-learning (2010)|van Hasselt 2010]]). The same decoupling idea reappears at scale as [[Deep RL with Double Q-Learning - DDQN (2015)|Double DQN]] and as the twin critics of modern continuous-control methods.

## Where tabular Q-learning ends

The table *is* the limitation: one cell per $(s,a)$ means no generalization between states and memory linear in $|\mathcal{S}||\mathcal{A}|$ — dead on arrival for pixels ([[Markov Decision Process|curse of dimensionality]]). Swapping the table for a network is the obvious move, and it walks straight into the [[Temporal Difference Learning|deadly triad]]: Q-learning is *the* maximally off-policy, maximally bootstrapped algorithm, so it's the triad's worst case (Baird's counterexample diverges on exactly this combination). Making that swap survivable took two specific inventions — the subject of [[Deep Q-Network]].

## Related

- [[Temporal Difference Learning]] — the machinery this specializes
- [[Bellman Equation]] — the optimality equation being sampled
- [[Deep Q-Network]] — this algorithm + a network + the stabilizers
- [[Exploration vs Exploitation]] — supplying the visit-everything precondition

## Sources

- [[Q-learning - Watkins & Dayan (1992)]] — the algorithm and convergence proof
- [[Double Q-learning (2010)]] — overestimation diagnosis and the double estimator
- [[Reinforcement Learning - An Introduction (1998)]] — the GPI framing

---
Part of the value-based branch — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
