---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [Q-learning vs policy gradient, value-based vs policy-based, off-policy vs on-policy, DDPG, TD3, SAC, offline RL]
---

# Value-Based vs Policy-Based RL

> **Where this sits.** The verdict note for the [[Reinforcement Learning|two bodies]]: which family owns which territory, with the evidence. Spoiler: the world is not "taken" by either — it is **partitioned by the price of data and the shape of the action space**, and the most-used continuous-control methods are hybrids that blur the line on purpose.

## First, ask the right question

"Q-learning vs policy gradient" is really three separate dividing lines stacked on top of each other:

1. **Off-policy vs on-policy** — can you learn from *other policies' data* (replay buffers, logged datasets, relabeled goals), or only from fresh rollouts? Off-policy buys enormous sample reuse but carries all three legs of the [[Temporal Difference Learning#The deadly triad|deadly triad]]; on-policy drops leg 3 and buys stability, paying in samples. **This is the load-bearing axis** — neither side repealed the trade-off; they picked different points on it
2. **Enumerable vs continuous/structured actions** — value-based control needs $\arg\max_a$; 18 buttons yes, joint torques no, 100k-token vocabularies beside the point
3. **Does a good policy already exist?** — if yes (a pretrained LLM), value-based methods would throw it away; policy methods fine-tune it directly

Hold a domain up against these three questions and its owner falls out.

## The hybrid middle: Q-learning in a trench coat

Before the territory map, one family that breaks the dichotomy — and dominates continuous control. **DDPG → TD3 → SAC** are called "actor-critics," but structurally they are **off-policy Q-learning with a learned argmax-solver**: replay buffer, target networks, bootstrapped Q-critics — the entire [[Deep Q-Network|DQN]] toolkit — plus an actor that exists *only because* $\max_a Q(s,a)$ can't be enumerated over continuous actions:

- **DDPG**: deterministic actor trained by the gradient of Q through the action; solved 20+ simulated physics tasks with one recipe, some from pixels ([[Continuous Control with Deep RL - DDPG (2015)|Lillicrap 2015]])
- **TD3**: [[Q-Learning|overestimation]] confirmed a third time — it bites in continuous actor-critic too — fixed with **twin critics taking the min** (Double-Q hardened into pessimism), delayed actor updates; beat SOTA in *every* environment tested ([[Addressing Function Approximation Error - TD3 (2018)|Fujimoto 2018]])
- **SAC**: adds the maximum-entropy objective (reward + policy entropy — [[Exploration vs Exploitation|Boltzmann exploration]] as part of the *goal*); **outperformed prior on-policy and off-policy methods in both sample efficiency and asymptotic performance**, unusually stable across seeds, and sample-efficient enough to train **real quadrupeds and dexterous hands** ([[Soft Actor-Critic (2018)|Haarnoja 2018]])

These are not [[Policy Gradient|REINFORCE-lineage]] methods — no likelihood-ratio gradients, no on-policy constraint. In spirit and machinery they belong to the value branch. Keep this in mind whenever "actor-critic" appears in a paper title: ask *which kind*.

## The territory map, domain by domain

**Value-based wins — samples expensive, actions discrete.** The replay buffer is a sample-efficiency machine (each transition trains the network many times — [[Deep Q-Network|the ~8× ratio]]); on-policy PPO uses a batch briefly and discards it. Evidence at both scales: [[Rainbow - Combining Improvements in Deep RL (2017)|Rainbow]] set SOTA on Atari in *data efficiency and final performance*; and in the canonical low-data regime — **Atari 100k**, two hours of gameplay — the strongest model-free agents have consistently been value-based, culminating in **BBF: super-human at 100k steps with no policy gradient in sight** ([[Bigger Better Faster - BBF (2023)|Schwarzer 2023]]).

**Value-based wins by necessity — offline RL (fixed dataset, zero interaction).** Medical records, robot logs, historical user data: no environment to roll out in. On-policy policy gradients **cannot even be estimated** here — their gradient is an expectation under the current policy's fresh trajectories, which don't exist. The field is Q-learning-style bootstrapping, tamed: naive DQN/DDPG fail on uncorrelated batches through **extrapolation error** (the max queries Q at actions absent from the data, where the network's guesses are fantasy — and nothing ever corrects them), so methods constrain learning toward the data (BCQ, then CQL/IQL) ([[Off-Policy Deep RL without Exploration - BCQ (2018)|Fujimoto 2018b]]).

**Value-based wins structurally — goal-conditioned sparse rewards.** [[Hindsight Experience Replay (2017)|HER]]'s trick — "I failed to reach goal A, but this trajectory *succeeded* at reaching where I ended up" — relabels stored experience with different goals and learns robot pushing/sliding/pick-and-place from **binary** rewards. Relabeled data is data collected under *other intentions*: only off-policy machinery can consume it. Bolting HER onto an on-policy method breaks the estimator.

**Hybrid (SAC/TD3) wins — real-hardware continuous control.** On a physical robot every sample costs wall-clock time and wear; SAC-class methods need several-fold fewer samples than PPO and are what made on-robot training feasible ([[Soft Actor-Critic (2018)|Haarnoja 2018]]). "PPO for robotics" usually means "we have a cheap simulator."

**Policy-based wins — cheap rollouts, reliability, tuning budget.** With a parallelizable simulator, sample cost stops binding and [[PPO]]'s virtues dominate: first-order simplicity, stability, one recipe across domains. The surprise result: even in cooperative *multi-agent* RL — off-policy's supposed home turf — tuned PPO (MAPPO) matched or beat off-policy methods **in final returns and sample efficiency both** ([[Surprising Effectiveness of PPO in Cooperative Multi-Agent Games (2021)|Yu 2021]]).

**Policy-based wins definitionally — LLM post-training.** The pretrained model *is* $\pi_\theta$; the update is a weighted cross-entropy; on-policy drops the triad leg that off-policy value learning spends all its engineering managing; and rollouts are cheap parallel inference — the sample-cost argument *inverts* ([[PPO|the full argument]]). No argmax over a vocabulary, no thrown-away pretraining.

**The third family lurking outside the dichotomy — model-based + search.** When data is scarcest of all, *learn the world and practice inside it*: EfficientZero (MuZero-style learned model + [[Monte Carlo Tree Search|MCTS]]) hit **194% mean human on Atari 100k — 500× less data than DQN needed** for similar performance ([[Mastering Atari with Limited Data - EfficientZero (2021)|Ye 2021]]). The sample-efficiency ladder runs: on-policy PG → off-policy value → model-based.

## ⚡ The scoreboard

| domain | owner | decisive factor | anchor result |
|---|---|---|---|
| discrete, cheap sim, max score | value-based | replay reuse | Rainbow: SOTA efficiency + final |
| discrete, ~2h of data | value-based (or model-based) | every sample precious | BBF superhuman @100k; EfficientZero 194% |
| offline / fixed dataset | value-based **by necessity** | PG can't be estimated | BCQ and descendants |
| goal-conditioned, sparse binary reward | off-policy value | relabeling needs off-policy | HER robot manipulation |
| real-robot continuous control | hybrid (SAC/TD3) | sample cost = hardware time | SAC on quadrupeds/hands |
| cheap simulator, want reliability | policy-based (PPO) | stability > thrift | PPO benchmarks; MAPPO |
| pretrained policy exists (LLMs) | policy-based | the policy is the asset | [[RLHF]] |

## The intuition to keep

One sentence per axis: **off-policy value learning buys sample efficiency by carrying the whole deadly triad and managing the risk; on-policy policy gradients buy stability by dropping a leg and paying in samples** — so the *price of data* decides the family. The *shape of the action space* decides whether an argmax is even available. And *whether a policy already exists* decides whether value learning would be construction or demolition. Today's most visible applications (cheap game simulators, LLM inference) happen to sit in policy-gradient territory — which is why it *looks* like policy learning won. Move the data price and the map redraws itself.

## Related

- [[Reinforcement Learning]] — the two bodies, defined
- [[Q-Learning]] · [[Deep Q-Network]] — the value branch
- [[Policy Gradient]] · [[PPO]] — the policy branch
- [[Temporal Difference Learning#The deadly triad|The deadly triad]] — the physics underneath the trade

## Sources

- [[Continuous Control with Deep RL - DDPG (2015)]] · [[Addressing Function Approximation Error - TD3 (2018)]] · [[Soft Actor-Critic (2018)]] — the hybrid middle
- [[Bigger Better Faster - BBF (2023)]] · [[Rainbow - Combining Improvements in Deep RL (2017)]] — value-based sample efficiency
- [[Off-Policy Deep RL without Exploration - BCQ (2018)]] — the offline territory
- [[Hindsight Experience Replay (2017)]] — the relabeling territory
- [[Surprising Effectiveness of PPO in Cooperative Multi-Agent Games (2021)]] — PPO's side of the ledger
- [[Mastering Atari with Limited Data - EfficientZero (2021)]] — the model-based caveat

---
Part of the RL cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
