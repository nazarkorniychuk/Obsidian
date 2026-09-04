---
type: concept
topics: [reinforcement-learning]
status: evergreen
created: 2026-08-29
aliases: [MCTS, UCT, AlphaGo, AlphaZero, MuZero, tree search]
---

# Monte Carlo Tree Search

> **Where this sits.** Decision-time planning: given a [[Markov Decision Process|model]] (rules or learned), spend compute *at the moment of acting* to look ahead. Its modern form is the engine of the AlphaGo lineage — and the cleanest existing demonstration that **search is a policy-improvement operator**.

## The problem: lookahead in trees too big to enumerate

With a model you can ask "what if?" — but game trees explode (Go: ~$b^d$ with $b \approx 250$, $d \approx 150$; more positions than atoms). Classical minimax needs a hand-crafted evaluation to cut depth, and full-width search wastes almost all its budget on hopeless branches. Two questions: *which branches deserve simulations*, and *how to evaluate leaves without an oracle*.

## UCT: a bandit at every node

The first question is literally the [[Exploration vs Exploitation|exploration-exploitation dilemma]], and **UCT** answers it with UCB ([[Bandit Based Monte-Carlo Planning - UCT (2006)|Kocsis & Szepesvári 2006]]): treat each tree node's children as bandit arms, and descend by

$$\text{child} = \arg\max_i \; \underbrace{\bar{V}_i}_{\text{avg. simulation value}} + \; c\,\underbrace{\sqrt{\tfrac{\ln N}{n_i}}}_{\text{UCB bonus}}$$

The loop, four phases per simulation: **select** (descend by UCB until leaving the tree) → **expand** (add a node) → **evaluate** (originally: random rollout to the end) → **backup** (propagate the result up the visited path). Asymmetry is the point: the tree grows *where the bandits direct it* — deep in promising lines, shallow in refuted ones — and UCT is provably consistent (converges to the optimal action). It immediately made computer Go programs master-level on small boards, but random rollouts remained a weak evaluator.

## The AlphaGo lineage: replace both weak parts with networks

- **AlphaGo** ([[Mastering the Game of Go - AlphaGo (2016)|Silver 2016]]): a **policy network** (supervised on human games, then [[Policy Gradient|self-play PG]]) narrows the search — expand only moves it proposes; a **value network** (trained on self-play outcomes) replaces random rollouts as the leaf evaluator. The networks alone, *zero search*, already matched full MCTS programs. Together: **99.8% vs other programs, 5–0 vs the European champion** — the first professional defeat in full-size Go, then Lee Sedol 4–1
- **AlphaGo Zero** ([[Mastering Go Without Human Knowledge - AlphaGo Zero (2017)|Silver 2017]]): delete the human data. One network, random initialization, rules only. The engine is a loop worth stating precisely because it *is* the [[Reinforcement Learning|master loop]]: **the search, guided by the current network, outputs better move probabilities than the raw network** (search = IMPROVE); the network is trained to predict the search's choices and the game outcomes (training = EVALUATE + compress the improvement); the stronger network makes the next search stronger (ACT). **Result: beat the champion-defeating AlphaGo 100–0** — human knowledge had been a ceiling, not a floor
- **AlphaZero** ([[A General RL Algorithm - AlphaZero (2018)|Silver 2018]]): the same loop, unchanged, mastered **chess, shogi, and Go — superhuman within 24 hours each**, beating Stockfish (decades of hand-crafted search engineering) while evaluating *thousands* of positions per second to Stockfish's tens of millions: learned evaluation buys selective depth over brute breadth
- **MuZero** ([[Mastering Atari Go Chess Shogi - MuZero (2019)|Schrittwieser 2019]]): delete the rules. Plan inside a **learned latent model** trained to predict only what the search consumes — policy, value, reward — never reconstructing the world. **Matched AlphaZero in the board games without knowing the rules, and set SOTA on Atari-57**, the domain where model-based planning had always failed. The bridge to [[Model-Based RL]] complete

## The recipe: an AlphaZero-style agent, ready to implement

The exact skeleton of [[A General RL Algorithm - AlphaZero (2018)|AlphaZero]] — what you'd write for any perfect-information board game, scale aside.

**The network.** One net, two heads: input = board tensor (current position + a few history planes, side to move); **policy head** → logits over an encoding of all possible moves; **value head** → $v \in [-1, 1]$, the predicted outcome *from the perspective of the player to move*. A small ResNet suffices for small games.

**The tree.** Each edge $(s, a)$ stores four numbers: visit count $N$, total value $W$, mean value $Q = W/N$, prior $P$ (the network's policy at $s$). One **simulation** = the four UCT phases, with the network replacing the rollout:

1. **Select** — from the root, repeatedly take the edge maximizing PUCT (UCB with the prior steering exploration):
$$a = \arg\max_a\; Q(s,a) \;+\; c_{puct}\, P(s,a)\, \frac{\sqrt{\sum_b N(s,b)}}{1 + N(s,a)}$$
   Store values from the perspective of the player to move and **negate at each ply** (your good position is my bad one). Unvisited edges have $Q = 0$ and a bonus ∝ $P$ — the network's favorite moves get tried first.
2. **Expand & evaluate** — at a leaf: *one* network call → $(p, v)$; create the children with priors $p$. No rollout: $v$ *is* the evaluation (terminal positions use the true game result instead).
3. **Backup** — walk the path back to the root: $N \mathrel{+}{=} 1$, $W \mathrel{+}{=} v$ (sign alternating per ply), $Q = W/N$.

Run a few hundred simulations per move (AlphaZero: 800). Then **act by visit count, not by $Q$** — visits are the robust statistic: play $a \sim N(a)^{1/\tau}$, with temperature $\tau = 1$ for the first ~30 plies (diverse openings), $\tau \to 0$ after (best move). Record $\pi \propto N$ at every position — *the search's verdict becomes the training target*.

**Root noise — the non-optional exploration piece.** At the root only, mix Dirichlet noise into the priors: $P \leftarrow 0.75\,P + 0.25\,\mathrm{Dir}(\alpha)$, with α scaled inversely to the game's branching factor (0.3 chess, 0.03 Go). Without it, deterministic self-play repeats the same game forever and the data collapses.

**The self-play loop** (the [[Reinforcement Learning|master loop]], literally):

1. Self-play games with current net + search; every position yields a triple $(s, \pi, z)$, where $z \in \{+1, 0, -1\}$ is the game's final outcome from that position's player's perspective
2. Train on a sliding window of recent positions: $\;\mathcal{L} = (v - z)^2 \;-\; \pi^\top \log p \;+\; c\|\theta\|^2$ — the value head regresses to *what actually happened*, the policy head distills *what the search concluded*
3. Stronger net → stronger search → better targets → repeat

**Two engineering notes that decide whether it runs at all:** batch leaf evaluations across many parallel games (a GPU fed one position at a time idles — use *virtual loss*: temporarily score an in-flight edge as a loss so concurrent simulations fan out over different lines), and after playing a move, **reuse the chosen child's subtree** as the next search's root.

### The recipe instantiated: AlphaZero playing chess

Every slot above, filled in with the real values from [[A General RL Algorithm - AlphaZero (2018)|the chess run]]:

- **The MDP.** State: the current position *plus the last 7 positions* — history planes are needed because bare-board chess isn't [[Markov Decision Process|Markov]] (threefold repetition, fifty-move rule). Actions: the legal moves. Reward: **zero on every move**, ±1/0 only at game's end, γ = 1 — the purest sparse-reward setting, made learnable by the value head carrying the outcome signal backward.
- **State encoding**: an 8×8×119 tensor — for each of 8 history steps, 12 binary piece planes (6 piece types × 2 colors) + 2 repetition-count planes; plus 7 constant planes: side to move, move count, 4 castling rights, the no-progress counter.
- **Action encoding**: an 8×8×73 stack = 4,672 logits — for each *from*-square: 56 "queen-move" targets (8 directions × up to 7 squares), 8 knight moves, 9 underpromotions. In any given position most are illegal: **mask illegal logits to −∞ and renormalize over the legal set.** This masking layer is where a real implementation spends its debugging time — and it's the only place the game's rules touch the network.
- **Network and search**: one ~20-block, 256-filter ResNet feeding both heads; **800 simulations per move** (a few tens of milliseconds) against Stockfish's tens of millions of positions per second; root Dirichlet α = 0.3; τ = 1 for the first 30 plies. Over-long games are scored as draws — an episode cap, like any other env.
- **Scale and outcome**: 44 million self-play games; 700k training steps at batch 4,096 (5,000 TPUs generating games, 64 training); surpassed Stockfish after ~4 hours of wall-clock and won the 100-game match **28–0 with 72 draws**. Along the way it *rediscovered* the standard human openings, played through them, and discarded several as inferior.

What this instance demonstrates: nothing chess-specific was engineered except the two encodings — the identical loop, with a different board tensor and move encoding, mastered shogi and Go. **The encodings are the interface; the recipe is the whole algorithm.** For your own game, the work is: design the state tensor, design the move encoding + legality mask, and the rest is the skeleton above verbatim.

## The idea to keep: search as amortized improvement

One sentence: **the network proposes, the search verifies and sharpens, and the network then learns to propose what the search concluded** — policy iteration where IMPROVE is a search procedure and the network is a cache of all past searches. This framing travels: the [[Practical Issues in Temporal Difference Learning - TD-Gammon (1992)|TD-Gammon]] self-play thread runs straight through it, and the contemporary echo is LLM **test-time compute** — spending inference-time search (long chains of thought, parallel sampling, tree-structured decoding) to outperform the raw policy, then distilling the results back into the model ([[RLVR]] is this loop with a verifier standing in for the game's win condition).

## Related

- [[Exploration vs Exploitation]] — UCB, transplanted into a tree
- [[Model-Based RL]] — background planning, the sibling; MuZero joins them
- [[Reinforcement Learning]] — the loop, made visible in silicon
- [[Value-Based vs Policy-Based RL]] — what a free perfect model buys

## Sources

- [[Bandit Based Monte-Carlo Planning - UCT (2006)]] — the algorithm
- [[Mastering the Game of Go - AlphaGo (2016)]] · [[Mastering Go Without Human Knowledge - AlphaGo Zero (2017)]] · [[A General RL Algorithm - AlphaZero (2018)]] · [[Mastering Atari Go Chess Shogi - MuZero (2019)]] — the lineage

---
Part of the model-based branch — hub: [[Reinforcement Learning]] · map: [[RL MOC]].
