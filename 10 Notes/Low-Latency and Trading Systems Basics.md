---
type: concept
topics: [swe-interview]
status: evergreen
created: 2026-09-13
aliases: [order book, low latency, trading systems, HFT basics]
---

# Low-Latency and Trading Systems Basics

> **Where this sits.** [[SWE Interview Prep]] — the Citadel-flavored layer. Interns aren't expected to know market microstructure, but coding rounds are often *dressed* in it (order books, market data, simulations), and showing you understand *why the domain forces the engineering* is a differentiator. Two things live here: the domain vocabulary, and the classic **order book design question**.

## The domain in one paragraph

Exchanges match buyers and sellers via a **central limit order book**. A **limit order** rests in the book ("buy 100 @ $10.00"); a **market order** executes immediately against the best resting orders. **Bids** (buys) sorted descending, **asks** (sells) ascending; **best bid < best ask**, the gap is the **spread**; matching is **price-time priority** — best price first, FIFO within a price level. A market maker (Citadel Securities' core business) quotes both sides and earns the spread, wearing inventory and adverse-selection risk — which is why *reacting to market data faster than competitors* is directly P&L, and why the firm cares about nanoseconds.

## The order book design question (know this cold)

**Requirements**: `add_order(id, side, price, qty)`, `cancel(id)`, `execute/match`, `best_bid/ask` — all fast; millions of ops/sec.

**The construction**:
- **Per side, a price-level structure**: sorted map `price → level` (C++ `std::map`, or two heaps if no cancels-at-level needed). Best price = first key: $O(1)$ amortized peek, $O(\log P)$ insert of a *new* level ($P$ = # active price levels, usually small)
- **Per level, a FIFO queue** of resting orders (doubly-linked list) — price-*time* priority is the queue order
- **Global hashmap `order_id → node pointer`** — makes `cancel` $O(1)$: look up, unlink from its level's list, decrement level volume, delete empty level
- **Match** (incoming aggressive order): walk best level's queue front-to-back consuming quantity, delete exhausted orders/levels, continue to next level while price crosses

The trio — sorted-map + FIFO lists + id-hashmap — is [[Data Structures Under the Hood|the LRU-cache trick]] scaled up: every structure covers another's weak operation. State the complexities unprompted: add $O(\log P)$, cancel $O(1)$, best quote $O(1)$, match $O(k)$ for $k$ orders consumed. **Escalations to expect**: "mostly cancels?" (hashmap path is why cancels dominate real books and must be $O(1)$); "top-of-book queries dominate?" (cache best pointers); "make it concurrent?" (single-writer-per-instrument beats locks — see below).

## Why the hot path looks the way it does

Everything follows from [[Concurrency and OS Basics|the latency table]] (RAM ~100× L1, network ~everything):

- **Contiguity over pointers**: arrays/intrusive structures, pre-allocated pools — no `new` on the hot path, no pointer-chasing cache misses. This is [[Cpp and Python Under the Hood|the C++ argument]] in its native habitat
- **No surprises over raw speed**: the enemy is *tail* latency — GC pauses, page faults, allocator spikes. Hence C++/Rust (no GC runtime), pinned threads, pre-touched memory
- **Single-writer designs**: one thread owns one instrument's book; inputs arrive via lock-free ring buffers (queues) — no mutexes on the hot path, no contention, and the code is *simpler* than fine-grained locking
- **Kernel bypass** (name-drop level): NIC → user space directly (DPDK-style), skipping kernel network-stack copies and syscalls; measure in nanoseconds, µs budgets end-to-end
- The intern-level takeaway sentence: *"asymptotics choose the structure; constants and tails choose the implementation"*

## Market data vs order entry (vocabulary)

Two separate flows: **market data in** (the exchange's feed of book updates/trades — huge fan-out, must be consumed without falling behind; a "simulate the book from a feed" problem is exactly the design above driven by messages) and **orders out** (risk checks, then the exchange gateway). An intern project usually lives beside these: tooling, simulation, research infrastructure — say that with comfort rather than pretending to HFT expertise.

## If asked "design a rate limiter" (the other domain-adjacent classic)

**Token bucket**: capacity $B$, refill rate $r$/sec; a request takes a token or is rejected; allows bursts up to $B$ while enforcing long-run rate $r$. $O(1)$ per request, tokens computed lazily from a timestamp — no background thread. Alternatives to name: fixed window (boundary spikes), sliding log (exact, memory-heavy). Concurrent version: atomic compare-and-swap on the token count.

## Related

- [[Data Structures Under the Hood]] — the structures composing the book
- [[Concurrency and OS Basics]] — latency numbers, single-writer reasoning
- [[Cpp and Python Under the Hood]] — why the hot path is C++
- [[Algorithmic Patterns for Interviews]] — heaps/sorted maps, the underlying patterns

---
Part of the interview-prep cluster — map: [[SWE Interview Prep]].
