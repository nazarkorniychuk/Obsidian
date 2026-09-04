---
type: project
topics: [reinforcement-learning]
status: planning
created: 2026-09-03
aliases: [chinese checkers bot, checkers AlphaZero]
---

# Chinese Checkers RL Bot

> **Goal.** Train a Chinese-checkers agent that beats me (and a decent heuristic) on a MacBook M3 Pro / 18 GB — no cloud, no human game data. Method: **AlphaZero-lite** — the recipe already written in [[Monte Carlo Tree Search#The recipe: an AlphaZero-style agent, ready to implement|the MCTS note]], instantiated for this game.
>
> Map: [[RL MOC]] · Status: **Planning**

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

- [ ] Rung 0: board + move generator + rules tests + heuristic opponent
- [ ] Rung 1: pure UCT, benchmark vs heuristic
- [ ] Rung 2: net + self-play loop + ELO ladder

## 🏁 End results

*(to fill at the end: final strength, wall-clock and FLOPs actually spent, what worked / what didn't, lessons to fold back into [[Monte Carlo Tree Search]] and [[Model-Based RL]])*

## Related

- [[Monte Carlo Tree Search]] — the recipe and the chess instantiation this project copies
- [[Model-Based RL]] — why a perfect model changes the method choice
- [[Value-Based vs Policy-Based RL]] — the territory argument behind the verdict table
- [[Exploration vs Exploitation]] — root Dirichlet noise, temperature

---
Project of the RL cluster — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
