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

**First, why the entries are noisy at all.** During training, every $Q(s', a')$ cell is a work in progress: an average over a *few* sampled rewards (the environment is random) plus bootstrap errors not yet ironed out. So at any moment, each entry = true value + some error — up or down, equally likely, honest on average.

**A thought experiment.** Ten slot machines, all secretly identical: every one has true average payout exactly **0**. You estimate each machine from a handful of pulls. Pure luck spreads your ten estimates around the truth:

$$-1.2,\; +0.8,\; -0.3,\; \mathbf{+1.5},\; -0.9,\; +0.2,\; -0.6,\; +1.1,\; -0.4,\; +0.7$$

Each individual estimate is fine — unbiased, right on average. Now ask the Q-learning question: *"how good is the best machine?"* — and answer it the Q-learning way, by taking the **max of the estimates**: $+1.5$. The true answer is 0. Where did $+1.5$ come from? **The max scanned the list and picked out the largest *error*.** Rerun the whole experiment with fresh luck and you'll get $+0.9$, $+2.1$, $+1.2$… essentially *always positive* — among ten coin-flip errors, at least one almost surely landed high, and the max hunts it down every time. Averaged over reruns, the max of ten ±1-scale errors sits around **+1.5**, not 0.

That's the mechanism in one sentence: **individual errors are symmetric, but the max converts them into a systematic upward push — it always chases the up-errors and never the down-errors.** More actions, or noisier estimates → bigger push. (The formal version: we want $\max_a$ of the *true* values, but compute the expected max of *estimates*, and $\mathbb{E}[\max_a \hat{Q}(a)] \ge \max_a \mathbb{E}[\hat{Q}(a)]$ always, with equality only in the noise-free case — [[Double Q-learning (2010)|van Hasselt 2010]].)

**Now look back at the update.** The target is $r + \gamma\, {\max_{a'} Q(s',a')}$ — literally the "how good is the best machine?" question, asked of noisy entries, *at every step, in every state*. So targets are systematically too high wherever estimates are still noisy — which during learning is everywhere. Two things stop it from washing out:

- **It compounds instead of averaging away.** In supervised learning, symmetric errors cancel over many samples. Here the inflated max is *written into* $Q(s,a)$ as its new value; later, $s$ appears as someone else's $s'$, and its inflated entry feeds *their* max. The bias is re-selected and re-copied at every step of the [[Bellman Equation|Bellman recursion]] — it travels upstream rather than cancelling
- **It's biggest exactly where you know least.** Entries with the fewest samples have the largest errors, hence the largest inflation. So the greedy policy is systematically pulled *toward the least-explored actions for phantom reasons* — uncontrolled fake optimism, where honest uncertainty should have counselled caution (contrast UCB in [[Exploration vs Exploitation]], where optimism is *calibrated* to the uncertainty and decays with data; this optimism is accidental and doesn't)

**Result:** in stochastic environments this is not a small correction — plain Q-learning can perform *very poorly*, and the overestimation is the identified cause ([[Double Q-learning (2010)|van Hasselt 2010]]).

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
