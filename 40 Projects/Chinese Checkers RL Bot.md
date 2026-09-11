---
type: project
topics: [reinforcement-learning]
status: doing
created: 2026-09-03
aliases: [chinese checkers bot, checkers AlphaZero]
---

# Chinese Checkers RL Bot

> **Goal.** Train a Chinese-checkers agent that beats me (and a decent heuristic) on a MacBook M3 Pro / 18 GB — no cloud, no human game data. Method: **AlphaZero-lite** — the recipe already written in [[Monte Carlo Tree Search#The recipe: an AlphaZero-style agent, ready to implement|the MCTS note]], instantiated for this game.
>
> Map: [[RL MOC]] · Status: **Doing** — Rungs 0–1 done, Rung 2 scaffolded

## 🗺 Planning

### The method verdict — which RL, and why

Every question answered from the cluster's own notes:

| question | verdict | why |
|---|---|---|
| [[Q-Learning]] / [[Deep Q-Network|DQN]]? | ❌ no | Q-learning shines with *no model* and a small action set. Here the action set is large and combinatorial (jump chains), and we own a **perfect free model** — the rules. Per [[Value-Based vs Policy-Based RL]], board games are not this family's territory |
| [[Policy Gradient]] / [[PPO]]? | ❌ not alone | PPO is the tool when you *can't* plan — a reactive policy trained from rollouts. With a perfect model, plain PPO throws away the game's biggest gift: lookahead. (A policy net still appears — inside the search) |
| [[Model-Based RL]]? | ✅ trivially | No model *learning* needed at all: the rules **are** the model, exact and free. That upgrade — learned model → perfect model — is what makes this the easy regime |
| [[Monte Carlo Tree Search]]? | ✅ the core | Two-player, zero-sum, perfect information, sparse terminal reward: exactly the AlphaZero regime. **Search is the IMPROVE operator; the net is the cache of past searches** |

**The chosen architecture: AlphaZero-lite.** Policy+value convnet, PUCT search, self-play training on $(s, \pi, z)$ triples. Nothing game-specific beyond two encodings (state tensor + move encoding with legality mask) — the [[Monte Carlo Tree Search#The recipe instantiated: AlphaZero playing chess|AlphaZero-chess instantiation]] is the template to imitate.

### The environment (the real design work)

- **Board**: the 121-cell star. Start with the **2-player variant** (opposite corners); 6-player is a later stretch. Embed the hex grid in a 2D array (axial/offset coordinates in a ~17×17 grid, dead cells masked) so convolutions apply
- **State tensor**: planes over the 121 cells — own pieces, opponent pieces, (later: a couple of history planes), side to move as a constant plane
- **Actions**: factorized **(from, to)** — two 121-way heads instead of one 121² head. Legality (step or jump *chain*) computed by BFS over jump graphs; **mask illegal logits to −∞ and renormalize** — as in chess, this mask is the only place the rules touch the net, and where the debugging time will go
- **Termination**: Chinese checkers can stall (pieces shuffling forever) → **ply cap ~300**, capped games scored by **progress** (sum of piece advancement toward the goal corner) rather than as bare draws — otherwise self-play learns to fortress
- **Reward**: ±1 terminal only (progress score only for capped games), γ = 1

### The rung ladder — build in testable stages

1. **Rung 0 — the engine**: board representation, move generator (jump-chain BFS), rules tests; a greedy **forward-distance heuristic** opponent as the first benchmark
2. **Rung 1 — pure UCT**: MCTS with random rollouts, no net ([[Bandit Based Monte-Carlo Planning - UCT (2006)|UCT]]). Should already beat the heuristic — this rung validates the env and the tree code *before* any learning exists
3. **Rung 2 — AlphaZero-lite**: add the net, self-play, training loop. Rung 1 becomes the sparring benchmark

Each rung is independently testable; never debug learning and rules at the same time.

### The network

- Small **ResNet, ~1–2M params** (a handful of blocks, 64–128 filters) — a convnet, not a transformer: at this data scale inductive bias wins (the ViT lesson), and the board is a spatial grid
- Two heads: policy (from-logits + to-logits over 121 cells each, masked) and value (tanh, outcome for the player to move)

### Hardware plan — M3 Pro, 18 GB

- **The one non-negotiable: batched leaf evaluation.** A GPU/ANE fed one position at a time idles; run **64–256 self-play games in parallel**, collect their leaf positions into a single batch per net call (virtual loss keeps concurrent simulations on distinct lines). This is the difference between weeks and hours
- Compute budget (estimated in chat, order-of-magnitude): ~1.5e18 FLOPs for a strong bot → at 0.5–1 TFLOPs sustained on Apple silicon, **~14–28 h of wall-clock** — feasible overnight ×2
- Small net + fp16 inference keeps memory trivial (18 GB is plenty); the replay window (last ~100k–500k positions) fits in RAM
- Watch the CPU side: move generation is Python-hostile — vectorize the rules in NumPy first; if self-play is still CPU-bound, port the move generator to a compiled extension (Rust/C) and keep everything else in Python

### Libraries

| layer | choice | note |
|---|---|---|
| tensors / training | **PyTorch (MPS backend)** | default; everything documented, works on Apple GPU. Alternative: **MLX** (Apple's framework — faster for small models on M-series, fewer batteries included) |
| board logic | **NumPy** (+ optional Rust/C extension later) | own engine; a custom env is *less* work than adapting gymnasium here |
| tests | pytest | rules tests are the foundation of the whole ladder |
| tracking | TensorBoard (or CSV + matplotlib) | loss curves, ELO ladder |

### Evaluation plan

- **ELO ladder**: periodic round-robins between checkpoints, the pure-UCT agent, and the heuristic — win-rate vs a *frozen* ladder is the only trustworthy progress signal (self-play loss is not)
- Milestones: Rung 2 beats Rung 1 ≥ 90% → beats me → stretch goals: 6-player, bigger net, more sims

## 🔨 Doing

*(log entries and checkboxes as work happens)*

- [x] Rung 0: board + move generator + rules tests + heuristic opponent
- [x] Rung 1: pure UCT, benchmark vs heuristic
- [ ] Rung 2: net + self-play loop + ELO ladder — *scaffolding done, training next*

**2026-09-08 — Rung 0 shipped** (`~/Documents/Projects/ChineseCheckers`). Engine as planned: 121-cell star in a 17×17 axial grid, flat `(121,)` int8 state, all rules reduced to precomputed `(121, 6)` STEP/JUMP tables (batch-ready), factorized `(from, to)` moves, jump-chain BFS, ply cap 300 with progress scoring. Random + greedy forward-distance agents; 20 rules tests pass (14 opening moves verified by hand). Plus a browser debug board (stdlib HTTP + one HTML page, zero game logic in JS) — play by hand, spar the greedy bot, watch self-play, see each jump chain drawn. Greedy vs greedy finishes in ~130 plies with a genuine filled-triangle win, so the anti-fortress cap rarely bites at this level.

**2026-09-09 — Rung 1 passed, the hard way.** Textbook UCT (uniform rollouts, c=1.4) lost 10/10 to greedy — root diagnosis: truncated-rollout values (±0.1–0.3) drowned under the UCB bonus → near-uniform visits → near-random play. Two rounds of fixes: (1) race-biased rollouts + gain-ordered expansion + progressive widening + c=0.7 → parity; (2) *sharper verdicts* — rollout depth 50→30, margin scale 20→10 → **80% vs greedy at 400 sims, 90% at 1200, and the search gradient restored (1200 beats 400 sims 8–2)**; control (old verdict params at 1200 sims) stuck at 50%, confirming evaluation quality, not budget, was the wall. Lesson for [[Monte Carlo Tree Search]]: c is a *reward-scale* parameter, and averaging more samples of a blurry evaluator buys precision, not truth.

**2026-09-09 — Rung 2 scaffolding shipped.** The recipe's skeleton, modular: `evaluators.py` (the (priors, value) socket — MCTS as amplifier around any policy/value pair), `search.py` (PUCT, root Dirichlet, temperature π for training), `encoding.py` (mover-relative 4×17×17 planes via 180° rotation; factorized from/to move indices + legality mask), `net.py` (ResNet ~0.5M @ 64×6, two heads, MPS-ready), `selfplay.py` ((s, π, z) + replay window), `train.py` (the master loop; smoke run: losses fall, checkpoints load back into the arena as `net:PATH` agents), `ladder.py` (round-robin + ELO, greedy anchored at 1000). 35 tests green. Next: a real overnight training run (sequential loop first), then the batched parallel engine.

**2026-09-10 — first real training night (runs/night1): learning, but slow.** 135 iterations, ~2,160 games (batched engine: 6 workers × 32 in-flight games, ~190 s/iter, ~25 plies/s sustained). What moved: search verdicts sharpened (π entropy 3.9 → 2.2), games began *finishing* (capped fraction 100% → ~40–60% after iter ~90), policy loss 3.96 → 3.09. What didn't: **0/20 vs greedy** (margin −32), and the value head is confident (|v| ≈ 0.43) but near-uncorrelated with outcomes in close games; the raw policy still lags its own search targets by ~0.9 nats. Beats random 9/10. Diagnosis: for the first ~80 iterations *every* game hit the ply cap, so all z labels were decided by hair-thin progress margins — the noisiest possible value signal (the failure mode predicted when reward shaping was discussed: cap-heavy shuffling starves the value head). Candidate fixes for night 2, in order: graded z for capped games (z = clamp(margin/K) instead of bare ±1 — honest labels for artificial endings), possibly a KataGo-style auxiliary margin head, and more sims per move now that the engine is ~12× faster.

## 🏁 End results

*(to fill at the end: final strength, wall-clock and FLOPs actually spent, what worked / what didn't, lessons to fold back into [[Monte Carlo Tree Search]] and [[Model-Based RL]])*

## Related

- [[Monte Carlo Tree Search]] — the recipe and the chess instantiation this project copies
- [[Model-Based RL]] — why a perfect model changes the method choice
- [[Value-Based vs Policy-Based RL]] — the territory argument behind the verdict table
- [[Exploration vs Exploitation]] — root Dirichlet noise, temperature

---
Project of the RL cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
