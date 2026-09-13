---
type: concept
topics: [swe-interview]
status: evergreen
created: 2026-09-13
aliases: [threads, deadlock, race condition, operating systems interview]
---

# Concurrency and OS Basics

> **Where this sits.** [[SWE Interview Prep]] — the fundamentals round, and the standard *constraint escalation* ("now make it thread-safe") of any design question. The answers here are short and canonical; the skill is delivering them precisely.

## Processes vs threads

**Process** = program instance with its *own* virtual address space, file descriptors, pages. **Thread** = execution stream *inside* a process: own stack + registers, **shared heap and globals**. Consequences, both directions: threads communicate by writing shared memory (fast, dangerous — races), processes by IPC/pipes/sockets (slow, safe); a crashed thread takes the process down, a crashed process doesn't take others. Context switch: threads cheaper (same address space — no TLB flush); process switch swaps page tables.

## Race conditions — the canonical example

`counter += 1` from two threads is **three** operations: load, add, store. Interleaving: both load 5, both store 6 — one increment lost. A **data race** = two threads access the same memory, at least one writes, no synchronization → in C++ that's undefined behavior outright, in every language it's nondeterministic corruption. Fixes, in escalating weight:

1. **Atomic** (`std::atomic<int>`, single CPU instruction with a lock prefix) — for single-variable counters/flags
2. **Mutex** — for multi-variable invariants: only one thread inside the critical section. C++ reflex: `std::lock_guard<std::mutex> g(m);` — RAII means no forgotten unlock on exceptions
3. **Message passing / immutability** — no shared mutable state, nothing to race on

## Deadlock — the four conditions (know them as a list)

Deadlock requires **all four**: (1) mutual exclusion; (2) hold-and-wait; (3) no preemption; (4) **circular wait**. Canonical instance: thread A holds lock 1 wants lock 2, thread B holds 2 wants 1. Break any one condition to prevent — the standard, cheap answer: **global lock ordering** (all threads acquire locks in one agreed order → no cycle possible). Alternatives: acquire-all-atomically (`std::scoped_lock(m1, m2)`), try-lock with backoff, lock-free structures. Related everyday bug: **livelock** (all backing off forever) and **starvation** (someone never scheduled).

## Condition variables — the producer/consumer template

For "wait until there's work" without spinning:

```python
with cv:                      # consumer
    while not queue:          # while, not if — spurious wakeups + races on re-acquire
        cv.wait()
    item = queue.popleft()

with cv:                      # producer
    queue.append(item)
    cv.notify()
```

The `while` is the interview point: between notify and wake-up, another consumer may have taken the item — recheck the predicate. This template *is* the thread-safe queue question; a bounded version adds a second condition (`not_full`).

## The GIL, placed here

Python threads never run bytecode in parallel ([[Cpp and Python Under the Hood|the GIL]]) — so in Python, threads are an *I/O concurrency* tool and `multiprocessing` is the CPU-parallelism tool. Note the race conditions above still exist in Python (the GIL switches between bytecodes; `+=` is several bytecodes).

## Memory hierarchy — the numbers that decide designs

| access | latency (order of magnitude) |
|---|---|
| L1 cache | ~1 ns |
| L2 | ~4 ns |
| main memory (RAM) | ~100 ns |
| SSD random read | ~100 µs |
| datacenter round trip | ~500 µs |
| spinning disk seek | ~10 ms |
| cross-continent round trip | ~150 ms |

The ratios are the content: **RAM is ~100× L1; disk is ~1000× RAM; network is ~everything.** Every caching decision, every "why is `vector` fast," every [[Low-Latency and Trading Systems Basics|low-latency]] argument is a restatement of this table. Corollary worth saying aloud: a cache line is ~64 bytes — sequential access gets the next 7 accesses ~free; pointer-chasing (linked lists, `shared_ptr` graphs) pays full price every hop.

## Virtual memory in three sentences

Each process sees a flat private address space; hardware + OS translate pages (~4 KB) to physical frames via page tables (TLB caches translations). Benefits: isolation, overcommit, memory-mapped files, swap. Page fault = translation miss → OS loads/allocates the page — which is why "memory" can silently cost a disk access.

## Related

- [[Cpp and Python Under the Hood]] — atomics, RAII locks, the GIL
- [[Low-Latency and Trading Systems Basics]] — this table, weaponized
- [[Data Structures Under the Hood]] — cache-friendly vs pointer-chasing structures

---
Part of the interview-prep cluster — map: [[SWE Interview Prep]].
