---
type: moc
topics: [swe-interview]
created: 2026-09-13
aliases: [Citadel Interview, SWE Interview MOC, interview prep]
---

# SWE Interview Prep

> Parent: [[Home]] · Target: **Citadel SWE internship**. Built like the other maps: notes hold the content, this page holds the process, the strategy, and the drill plan.

## 🎯 The Citadel pipeline (what each stage actually tests)

| stage | format | what it tests | note to drill |
|---|---|---|---|
| **Online assessment** | HackerRank, ~66 min, 2 LC medium/hard, hidden tests | pattern recognition under time; *suboptimal complexity fails* — brute force won't pass hidden tests | [[Algorithmic Patterns for Interviews]] |
| **Phone screen** | ~45 min = 15 behavioral + 30 technical | one clean problem end-to-end: narrate, code, test, state complexity | Patterns + [[Data Structures Under the Hood]] |
| **Superday** | ~3 back-to-back rounds | LC medium/easy-hard with **escalating constraints**; implementation & class design; CS fundamentals; often a probability round | everything below |

Timeline ~2–6 weeks. The single most Citadel-specific dynamic: **interviewers escalate constraints** — you solve it, then "now inputs are 100× larger," "now it must be O(1) per query," "now it's concurrent." Always know your solution's *next-faster* variant and its trade-offs before declaring done.

## 📖 The notes

- [[Algorithmic Patterns for Interviews]] — 🔑 the core: ~14 patterns, each with recognition cues + template + classic problems
- [[Data Structures Under the Hood]] — what interviewers probe *after* you use a structure: amortized doubling, hash internals, heaps, LRU design
- [[Cpp and Python Under the Hood]] — language internals questions: RAII, move semantics, vtables; GIL, dict internals, gotchas
- [[Concurrency and OS Basics]] — threads, races, deadlock's four conditions, latency numbers
- [[Probability and Brainteasers]] — linearity of expectation, first-step analysis, Bayes, sampling algorithms (you have the [[18.102 Functional Analysis|math]] — this is the *interview dialect* of it)
- [[Low-Latency and Trading Systems Basics]] — the Citadel flavor: order book design (the classic question), market data, why cache locality is money

## 🥊 The drill plan

1. **Daily**: 2 timed problems (45 min cap each) from the pattern currently weakest — simulate OA conditions: no IDE autocomplete, test before submitting
2. **Per problem, the ritual**: restate + clarify constraints (input size *tells you* the target complexity: $n \le 10^5 \Rightarrow O(n \log n)$; $n \le 20 \Rightarrow$ bitmask/backtracking) → brute force stated aloud → optimize → code → walk through one example + one edge case → complexity, time and space
3. **Weekly**: one mock interview out loud (the narration is a separate skill from the solving); one class-design problem (LRU, rate limiter, order book)
4. **Before superday**: re-read all six notes; drill the [[Probability and Brainteasers|probability classics]] table until instant

## 🗣 Behavioral (the 15 minutes people skip)

- Three STAR stories prepared: a hard technical problem, a conflict/feedback story, a project you drove end-to-end (the checkers RL bot in [[Chinese Checkers RL Bot|40 Projects]] is genuinely strong material: self-directed, measurable, systems + ML)
- "Why Citadel": performance culture + engineering at the point where microseconds are P&L — connect to *your* taste for rigor (the math vault is evidence)
- Always have 2 questions for the interviewer: their team's latency/scale constraints; what separates interns who get return offers

## Process sources

[Interview Query — Citadel SWE guide](https://www.interviewquery.com/interview-guides/citadel-software-engineer) · [TechPrep — Citadel process](https://www.techprep.app/blog/citadel-interview-process) · [AlgoMonster — Citadel guide](https://algo.monster/interview-guides/citadel) · [techinterview.org — Citadel Securities](https://www.techinterview.org/companies/citadel-securities/) · [Glassdoor — Citadel SWE intern](https://www.glassdoor.com/Interview/Citadel-Software-Engineering-Intern-Interview-Questions-EI_IE14937.0,7_KO8,35.htm)
