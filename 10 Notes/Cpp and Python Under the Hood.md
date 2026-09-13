---
type: concept
topics: [swe-interview]
status: evergreen
created: 2026-09-13
aliases: [C++ interview questions, Python internals, RAII, move semantics, GIL]
---

# C++ and Python Under the Hood

> **Where this sits.** [[SWE Interview Prep]] — the language-internals round. Citadel is a C++ shop; claiming C++ on your resume invites these directly, and even Python-only interviews probe "what actually happens when…". Rule: never claim a language deeper than you can answer this note.

## C++ — the seven questions that cover 90%

**1. Stack vs heap.** Locals live on the stack — allocation = bumping a pointer, freed on scope exit, fast and cache-warm. `new` allocates on the heap — general-purpose allocator, must be freed, can fragment. Interview reflex: heap allocation in a hot loop is a performance bug.

**2. RAII — the C++ idea.** *Resource Acquisition Is Initialization*: tie every resource (memory, file, lock) to an object's lifetime — constructor acquires, **destructor releases, automatically, on every exit path including exceptions**. This is why C++ has no `finally`: `std::lock_guard`, `std::unique_ptr`, `std::vector` *are* the finally.

**3. Smart pointers.** `unique_ptr<T>`: sole owner, zero overhead, move-only — the default. `shared_ptr<T>`: reference-counted via a heap-allocated **control block**; copying bumps an atomic count; last owner destroys. `weak_ptr`: observes without owning — **breaks reference cycles** (two `shared_ptr`s pointing at each other never free; the classic question). Rule of thumb to say: ownership should be a tree of `unique_ptr` with `shared_ptr` only when lifetime is genuinely shared.

**4. Move semantics.** Copying a `vector` duplicates the buffer; **moving steals it** — the source's pointer is transferred, source left empty; $O(n) \to O(1)$. `std::move(x)` performs no move: it's a *cast* to rvalue reference, marking $x$ as safe-to-plunder. **Rule of five**: define one of destructor/copy-ctor/copy-assign/move-ctor/move-assign and you should consider all five (or `= default`/`= delete` explicitly). Best: rule of zero — let members manage themselves.

**5. Virtual dispatch.** `virtual` → object carries a hidden **vptr** to its class's **vtable** (array of function pointers); calls indirect through it — dynamic dispatch at the cost of one indirection and no inlining. Non-virtual calls resolve at compile time. Must-know: a base class deleted through a base pointer needs a **virtual destructor**, else derived members leak (UB). Follow-up bait: virtual calls in constructors dispatch to the *base* version (derived part doesn't exist yet).

**6. Undefined behavior — name three.** Dangling pointer/reference (returning a ref to a local; iterator use after `vector` reallocation); out-of-bounds access; signed integer overflow; data races. The point to make: UB isn't "it crashes" — the compiler *assumes it never happens* and optimizes accordingly, so symptoms appear far from causes.

**7. STL complexities + cache.** `vector` push_back amortized $O(1)$ (may invalidate iterators); `map` = red-black tree, $O(\log n)$, ordered; `unordered_map` = hash, $O(1)$ average with rehash spikes. And the answer that marks you as hire-worthy: for small/medium $n$, `vector` linear scan beats `map`/`list` — [[Low-Latency and Trading Systems Basics|cache locality]] dominates asymptotics.

## Python — what actually happens

**Everything is a heap object.** `x = 5` binds a name to a heap-allocated `int` object (refcount, type pointer, value). Assignment copies *references*, never objects — hence the aliasing classic: `b = a; b.append(1)` mutates "both." `is` compares identity, `==` value (small-int caching makes `a is b` true for −5..256 — trivia bait).

**Memory management**: reference counting (immediate reclamation at zero) + a cyclic garbage collector for reference cycles. `del` decrements, doesn't free.

**The GIL.** One interpreter lock: only one thread executes Python bytecode at a time ⇒ **threads don't parallelize CPU-bound Python** — they interleave. Threads still win for I/O-bound work (the lock is released during blocking I/O and inside C extensions like NumPy). CPU parallelism → `multiprocessing` (separate interpreters, IPC cost) or native-extension code. Full answer connects to [[Concurrency and OS Basics]].

**Container internals**: `list` = dynamic array *of pointers* (append amortized $O(1)$, `insert(0, x)` is $O(n)$ — use `deque`); `dict`/`set` = open-addressing hash, insertion-ordered since 3.7; strings immutable ⇒ `s += t` in a loop is $O(n^2)$, use `"".join(parts)`.

**The gotcha list** (each a real screening question):
- **Mutable default argument**: `def f(x, acc=[])` — the list is created *once at def time* and shared across calls. Fix: `acc=None` + create inside
- Late-binding closures: `[lambda: i for i in range(3)]` all return 2 — bind with `lambda i=i:`
- `[[0]*3]*3` makes three references to *one* row
- Generators are single-pass and lazy; `range` is a lazy sequence, not a list
- Integer division `//` floors toward $-\infty$ (so `-7 // 2 == -4`); C++ truncates toward 0 — off-by-one bait in ported code

## The cross-language one-liner

If asked "C++ or Python and why," the strong answer is a trade, not a preference: *Python for iteration speed and expressiveness where milliseconds don't matter; C++ where the memory layout and the absence of a runtime are the product — and I know which regime I'm in.* (For where that regime lives at Citadel: [[Low-Latency and Trading Systems Basics]].)

## Related

- [[Data Structures Under the Hood]] — the layer above
- [[Concurrency and OS Basics]] — GIL, threads, races
- [[Low-Latency and Trading Systems Basics]] — why C++ owns the hot path

---
Part of the interview-prep cluster — map: [[SWE Interview Prep]].
