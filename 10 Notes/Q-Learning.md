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

Each individual estimate is fine — unbiased, right on average. Now ask the Q-learning question: *"how good is the best machine?"* — and answer it the Q-learning way, by taking the **max of the estimates**: $+1.5$. The true answer is 0. Where did $+1.5$ come from? **The max scanned the list and picked out the largest *error*.**

Was that a fluke of this particular list? No — a counting argument shows the max is *almost never* right. Each error is equally likely to land above or below zero. For the max to come out negative (or even just near the true 0), **all ten** estimates would have to land below zero simultaneously: probability $(1/2)^{10} \approx 0.1\%$. So ~99.9% of the time at least one estimate lands above zero — and the max reports *at least* that. Worse: the max doesn't grab a random positive error, it grabs the **largest** of the ~5 positive ones, which is typically far up the noise scale. Redo the ten estimates with fresh pulls as often as you like — the max comes out $+0.9$, $+2.1$, $+1.2$, …: positive nearly every single time, averaging around $+1.5$ for ten ±1-scale errors. A quantity whose true value is **0** gets reported as **≈ +1.5**, reliably.

That's the mechanism in one sentence: **individual errors are symmetric, but the max converts them into a systematic upward push — it always chases the up-errors and never the down-errors.** More actions, or noisier estimates → bigger push. (The formal version: we want $\max_a$ of the *true* values, but compute the expected max of *estimates*, and $\mathbb{E}[\max_a \hat{Q}(a)] \ge \max_a \mathbb{E}[\hat{Q}(a)]$ always, with equality only in the noise-free case — [[Double Q-learning (2010)|van Hasselt 2010]].)

**Now look back at the update.** The target is $r + \gamma\, {\max_{a'} Q(s',a')}$ — literally the "how good is the best machine?" question, asked of noisy entries, *at every step, in every state*. So targets are systematically too high wherever estimates are still noisy — which during learning is everywhere. Two things stop it from washing out:

- **It compounds instead of averaging away.** In supervised learning, symmetric errors cancel over many samples. Here the inflated max is *written into* $Q(s,a)$ as its new value; later, $s$ appears as someone else's $s'$, and its inflated entry feeds *their* max. The bias is re-selected and re-copied at every step of the [[Bellman Equation|Bellman recursion]] — it travels upstream rather than cancelling
- **It's biggest exactly where you know least.** Entries with the fewest samples have the largest errors, hence the largest inflation. So the greedy policy is systematically pulled *toward the least-explored actions for phantom reasons* — uncontrolled fake optimism, where honest uncertainty should have counselled caution (contrast UCB in [[Exploration vs Exploitation]], where optimism is *calibrated* to the uncertainty and decays with data; this optimism is accidental and doesn't)

**Result:** in stochastic environments this is not a small correction — plain Q-learning can perform *very poorly*, and the overestimation is the identified cause ([[Double Q-learning (2010)|van Hasselt 2010]]).

## The fix: decouple selection from evaluation

Recall what the **target** is: in every TD update, it's the number you nudge $Q(s,a)$ *toward* — "what one step of reality says this entry should be." Q-learning's target is $r + \gamma \max_{a'} Q(s',a')$, and the whole problem above lives inside that max.

**The diagnosis: the max is secretly doing two jobs with one noisy table.** Unpack $\max_{a'} Q(s',a')$ into its two steps:

1. **Selection** — *which* action looks best: $a^* = \arg\max_{a'} Q(s',a')$
2. **Evaluation** — *how good* is that action: read off $Q(s', a^*)$

Standard Q-learning uses the **same table for both**. That's precisely what lets one lucky error pay twice: the entry with the biggest upward error *wins the selection* (step 1), and then *its own inflated number* is used as the evaluation (step 2). The +1.5 machine from the thought experiment gets picked *because* it reads +1.5, and then contributes +1.5 to the target.

**The fix: give each job to a different, independently-noisy table.** Keep two tables $Q^A$ and $Q^B$ — two separate estimates of the same $Q^*$, kept independent by training each on a *different random half of the experience* (each step, a coin flip decides which table gets updated). Independence is the entire point: machine #4 being $Q^A$'s lucky one says *nothing* about what $Q^B$ thinks of machine #4.

One full update step, concretely. You experience $(s, a, r, s')$ and the coin says "update $A$":

1. **$A$ selects:** $a^* = \arg\max_{a'} Q^A(s', a')$ — A picks the action *it* believes is best
2. **$B$ evaluates:** $\text{target} = r + \gamma\, Q^B(s', a^*)$ — but the *number* comes from B's entry for that action
3. **A updates toward it:** $Q^A(s,a) \mathrel{+}= \alpha\,[\text{target} - Q^A(s,a)]$

(If the coin says "update $B$", swap every A and B.) Both tables keep improving; when you need to act, use their sum or average.

**Run the slot machines through it.** All ten machines truly worth 0. $Q^A$'s noisy estimates are the list from before, so step 1 picks machine #4 — the one whose *A-error* happens to be +1.5. Now step 2 asks $Q^B$: "what's machine #4 worth?" But $Q^B$'s error on machine #4 is *independent* of A's — as likely $-0.4$ as $+0.6$, and **zero on average**. So the number entering the target is ≈ 0, the truth — not +1.5. A's selection was still fooled (it picked an essentially random machine among equals — harmless here, since they're equal), but *being lucky in A no longer pays, because A's luck doesn't transfer to B*. The systematic upward push is gone.

**Results** ([[Double Q-learning (2010)|van Hasselt 2010]]): the double estimator eliminates the overestimation — trading it for possible mild *under*estimation (if A's pick was genuinely suboptimal, B honestly scores a suboptimal action, landing slightly below the true max); the algorithm **provably converges to the optimal policy** under the same conditions as Watkins; and it dramatically outperforms plain Q-learning wherever rewards are noisy. The decoupling idea became a standard part: [[Deep RL with Double Q-Learning - DDQN (2015)|Double DQN]] reuses it at scale with a cheaper compromise (online network selects, target network evaluates — not fully independent, but independent *enough*), and the twin critics of modern continuous-control methods are the same trick again.

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
