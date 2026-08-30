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
