---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [model-based reinforcement learning, MBRL, world models, Dyna, Dreamer]
---

# Model-Based RL

> **Where this sits.** Everything so far was **model-free**: never estimate $P$ or $R$, let real transitions stand in for expectations. Model-based RL takes the other fork from the [[Markov Decision Process|MDP note's map of methods]]: **learn the model, then generate your own experience inside it.** It's the top rung of the sample-efficiency ladder — and it has one characteristic disease.

## The motivation: real samples are the expensive part

Every method in the cluster ultimately pays in environment interactions — [[Deep Q-Network|days of gameplay]], [[Generalized Advantage Estimation|weeks of simulated locomotion]]. But interactions carry more information than model-free methods extract: a transition $(s, a, r, s')$ teaches [[Q-Learning]] about *one cell*, yet it's also evidence about *how the world works*. Model-based RL cashes that in: fit $\hat{P}, \hat{R}$ to experience, then produce **unlimited imagined transitions for free** and train on those. If the model is good, the real-sample bill collapses.

## Dyna: the founding loop

The architecture everything descends from ([[Dyna - Integrated Learning Planning Reacting (1990)|Sutton 1990]]): interleave three processes —

1. **Act** in the world; learn from the real transition (ordinary Q-learning)
2. **Update the model** on that same transition
3. **Plan**: draw imagined transitions from the model and feed them to the *identical* learning rule — "planning is learning from hallucinated experience"

Real and imagined data are indistinguishable to the learner. **Results:** on navigation tasks, Dyna converges in far fewer *real* steps than model-free learning, and works even with the imperfect, evolving models that learning produces. Two modern echoes worth noticing: a **replay buffer is Dyna with a non-parametric model** (the stored data *is* the model — replaying it is planning with an empirical world model), and Dreamer-style imagination training is Dyna with a deep latent model.

## The disease: compounding model error

Why doesn't everyone do this? Because imagined rollouts feed the model's own slightly-wrong predictions back as inputs: step 1 is off by ε, step 2 starts from a state the model invented, and error **compounds exponentially with rollout length** until the policy is being trained on fiction — and, worse, the policy *actively seeks* the model's errors (optimistically wrong regions look like high reward: the [[Q-Learning|max-selects-errors]] mechanism, now against a learned model). The disciplined fix ([[When to Trust Your Model - MBPO (2019)|MBPO]]): **many short rollouts branched from real states** — never let imagination stray far from data — with an analysis quantifying how much model usage measured generalization justifies. **Result:** the first method to beat model-based sample efficiency *and* match the best model-free asymptotic performance — the combination that had eluded the field (fast early, previously plateaued low).

## World models: train the agent inside the dream

The pixel-era version: learn a compact **latent** dynamics model (encode observations, predict forward in latent space) and train actor + critic **entirely on imagined latent trajectories** ([[Mastering Diverse Domains - DreamerV3 (2023)|the Dreamer line]]: PlaNet → Dreamer → V2, the first world-model agent at human-level Atari, → V3). **Results for V3:** outperforms specialized methods across **150+ tasks with one configuration**, and is the **first algorithm to collect diamonds in Minecraft from scratch** — no human data, no curricula: long-horizon sparse-reward exploration from pixels. Latent imagination sidesteps part of the compounding problem (no need to predict pixels, only decision-relevant state) — the same insight [[Mastering Atari Go Chess Shogi - MuZero (2019)|MuZero]] pushes to its limit: learn a model that predicts *only what planning consumes* (policy, value, reward), nothing else.

## When to reach for it

The [[Value-Based vs Policy-Based RL|sample-efficiency ladder]]: on-policy PG → off-policy value → **model-based**, which wins when data is scarcest — [[Mastering Atari with Limited Data - EfficientZero (2021)|EfficientZero]]'s 194% mean-human Atari-100k with 500× less data than DQN is the flagship number. The trade: model bias replaces sample cost, and exploitable model error is a standing risk. Two distinct ways to *use* the model, worth keeping separate: **background planning** (Dyna/Dreamer — imagined data trains a policy offline) vs **decision-time planning** (search at the moment of acting — [[Monte Carlo Tree Search]]).

## Related

- [[Markov Decision Process]] — what "the model" is
- [[Monte Carlo Tree Search]] — decision-time planning, the sibling
- [[Value-Based vs Policy-Based RL]] — where model-based sits in the territory map
- [[Q-Learning]] — the learner inside Dyna-Q

## Sources

- [[Dyna - Integrated Learning Planning Reacting (1990)]] — the founding loop
- [[When to Trust Your Model - MBPO (2019)]] — compounding error, short rollouts
- [[Mastering Diverse Domains - DreamerV3 (2023)]] — imagination training at maturity
- [[Mastering Atari Go Chess Shogi - MuZero (2019)]] — value-equivalent models
- [[Mastering Atari with Limited Data - EfficientZero (2021)]] — the sample-efficiency flagship

---
Part of the model-based branch — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
