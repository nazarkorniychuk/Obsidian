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

## The recipes: how you'd actually build one

**Dyna-Q — the tabular rung (an afternoon of code).** The model can literally be a hash map: after each real transition, $\text{model}[s,a] \leftarrow (r, s')$ (last-observed works for deterministic worlds; keep counts/averages for stochastic ones). The loop:

1. Act ε-greedy from $Q$; observe $(s, a, r, s')$; one ordinary [[Q-Learning]] update
2. Store the transition in the model
3. **Plan, $n$ times** ($n$ = 5–50): pick a random *previously visited* $(s,a)$, look up the model's $(r, s')$, apply *the identical Q-learning update* to the imagined transition

Step 3 is the entire idea — same update rule, hallucinated data — and $n$ is the knob that converts compute into sample efficiency.

**MBPO-style — the deep rung (continuous control).** Four design decisions carry it, and every one is an answer to the compounding-error disease above ([[When to Trust Your Model - MBPO (2019)|MBPO]]):

1. **The model is an ensemble** (~7 nets, best ~5 kept by validation loss), each predicting a Gaussian over the *change* $\Delta s = s' - s$ and the reward — deltas, not raw next states, so the net learns dynamics rather than copying its input. The ensemble is the uncertainty meter: where members disagree, the model doesn't know
2. **Imagine only short rollouts (a few steps), branched from random *real* states in the replay buffer** — never from the initial state, never far from data. Each imagined step queries a randomly chosen ensemble member, so model uncertainty shows up as visible noise instead of one net's confident fiction
3. **Digest the imagined data with an off-policy learner** (SAC): imagined transitions go to their own buffer, and the learner takes many gradient steps per real environment step — synthetic experience is unlimited, so spend it
4. **Refresh on a schedule**: retrain the model every few hundred real steps and regenerate the rollouts — imagined data from a stale model is exactly the fiction the disease warns about

The transferable rules of thumb, even outside MBPO: predict deltas; ensemble for uncertainty; imagination branches *from* data and stays *near* data; an off-policy learner to absorb the synthetic experience.

### The recipe instantiated: MBPO on Hopper

The canonical benchmark run from [[When to Trust Your Model - MBPO (2019)|the paper]], slot by slot:

- **The env.** MuJoCo Hopper: a one-legged planar robot that must hop forward. Observation: an 11-dim real vector (joint angles and velocities of torso, thigh, leg, foot). Action: 3 joint torques in $[-1, 1]$. Reward, dense: forward velocity + a small alive bonus − a control cost. Episodes end at 1,000 steps *or the moment the robot falls* — so the first thing every policy learns is simply not falling.
- **The model, concretely**: 7 MLPs (4 hidden layers × 200 units each), input = 14 numbers (11 state + 3 action), output = mean and variance of a Gaussian over 12 numbers (11-dim $\Delta s$ + reward). Trained by maximum likelihood on the entire real buffer; the 5 with best held-out loss are the elites that get queried.
- **The imagination schedule**: every 250 real steps, retrain the ensemble; branch ~400 imagined rollouts *per real step* from random buffer states. On Hopper the rollout length is **k = 1** — a single imagined step! — while harder bodies (Ant, Humanoid) grow k linearly toward ~25 as the model earns trust over training. The volume does the work: hundreds of one-step branches per real step, not one long dream.
- **The learner**: SAC taking ~20 gradient steps per real environment step (vanilla SAC takes 1) — the unlimited imagined buffer is what licenses being that update-greedy.
- **The result**: SAC's asymptotic Hopper score with roughly an order of magnitude fewer real transitions; the bill moves from environment interaction to model training and rollout generation — pure compute.

What this instance demonstrates: model error is *managed, never eliminated*. The ensemble stays visibly imperfect all the way through, and every design choice — deltas, elites, k = 1 branches anchored at real states, scheduled refresh — is about **bounding how much any single wrong prediction can matter**, not about making the model right.

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
