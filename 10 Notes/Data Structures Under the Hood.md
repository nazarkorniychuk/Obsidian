---
type: concept
topics: [swe-interview]
status: evergreen
created: 2026-09-13
aliases: [hashmap internals, data structure internals, LRU cache]
---

# Data Structures Under the Hood

> **Where this sits.** [[SWE Interview Prep]] layer two: after you *use* a structure, the follow-up is "how does it work, and when does it fail?" The pattern of every answer: what's in memory → why the advertised complexity holds → the case where it breaks.

## Dynamic array (Python `list`, C++ `vector`)

Contiguous block + spare capacity; append writes into the spare; when full, allocate ~2× (C++; Python ~1.125×) and copy. **Amortized $O(1)$ append**: doubling from size 1 to $n$ copies $1 + 2 + 4 + \cdots + n \approx 2n$ elements total — $O(1)$ per append *on average*, though any single append can be $O(n)$. Insert/delete at front = $O(n)$ shift (use a deque). Contiguity is why iteration is fast: cache lines. C++ trap: `push_back` can reallocate → **iterators/pointers invalidated**.

## Hash map (Python `dict`, C++ `unordered_map`)

- **In memory**: array of buckets; key → `hash(key) mod capacity` → bucket. Collisions: chaining (C++: linked lists per bucket) or open addressing (Python: probe sequence to next slots)
- **Why $O(1)$**: with a good hash and **load factor** (entries/buckets) kept below a bound (~0.66 Python, 1.0 C++ default), expected chain length is constant. Exceed it → **rehash**: allocate bigger table, reinsert all — amortized like the dynamic array
- **When it breaks**: adversarial keys all hashing to one bucket → $O(n)$ per op (real DoS vector; Python randomizes string hashes for this reason). Also: keys must be immutable/hashable — mutating a key after insert silently loses it
- **What a hashmap can't do**: range queries, min/max, predecessor, ordered iteration — that's the ordered map's territory
- Python 3.7+ dicts preserve *insertion* order (a compact-array implementation detail that became a guarantee) — not *sorted* order

## Ordered map / balanced BST (C++ `map`, `set`)

Red-black tree: $O(\log n)$ insert/erase/find, but *sorted iteration, floor/ceiling, range queries* — the answers a hashmap can't give. Interview cue: "closest value ≤ x", "count in range", "iterate in order under inserts" → ordered map. Python has no built-in; `sortedcontainers.SortedList` or `bisect` on a list (insert $O(n)$ but fine if inserts are rare). Trade vs hashmap: $\log n$ vs 1, but ordered structure and no rehash spikes.

## Heap (`heapq`, `priority_queue`)

Complete binary tree **embedded in an array**: children of $i$ at $2i{+}1, 2i{+}2$ — no pointers, cache-friendly. Push/pop = sift up/down $O(\log n)$; **peek $O(1)$**; build-heap from array $O(n)$ (not $n \log n$ — sift-down costs sum telescopically). Not a sorted structure: only the root is guaranteed. `heapq` is min-only → push negatives for max-heap; tie-break by pushing tuples `(key, counter, item)`.

## Deque and monotonic queue

Doubly-linked blocks (Python) — $O(1)$ push/pop both ends. **Monotonic deque** = sliding-window max in $O(n)$: front holds the current max's index; pop back while new element is larger; pop front when it leaves the window.

## Trie

Tree keyed by characters; word membership/prefix queries in $O(\text{len})$, independent of dictionary size. Node = dict of children + end-flag. Cue: "many words, prefix queries, autocomplete, word search II." Memory-heavy — mention that as the trade.

## LRU cache — the classic design question

**Requirement**: `get` and `put` in $O(1)$, evict least-recently-used at capacity. **The construction to know cold**: hashmap `key → node` + doubly-linked list in recency order:
- `get`: hashmap lookup → unlink node → move to front → return value
- `put`: exists → update + move to front; else insert at front; over capacity → drop tail node *and its hashmap entry*
Every operation is a pointer splice + a dict op — $O(1)$. (Python shortcut to *mention*, then implement raw anyway: `OrderedDict.move_to_end`.) Variants: LFU (adds count-buckets), TTL cache (adds expiry check on read).

## The complexity card

| structure | access | search | insert | delete | notes |
|---|---|---|---|---|---|
| dynamic array | $O(1)$ | $O(n)$ | $O(1)$ amort. end / $O(n)$ mid | $O(n)$ | cache king |
| hashmap | — | $O(1)$ avg, $O(n)$ worst | $O(1)$ avg | $O(1)$ avg | no order |
| balanced BST | — | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | sorted iteration, ranges |
| heap | peek $O(1)$ | $O(n)$ | $O(\log n)$ | pop $O(\log n)$ | build $O(n)$ |
| deque | ends $O(1)$ | $O(n)$ | ends $O(1)$ | ends $O(1)$ | sliding windows |
| linked list | $O(n)$ | $O(n)$ | $O(1)$ w/ node | $O(1)$ w/ node | splice; cache-hostile |
| trie | — | $O(L)$ | $O(L)$ | $O(L)$ | prefix queries |

The C++ addendum interviewers like: `vector` beats `list` in practice *even for middle inserts* at moderate sizes — pointer chasing destroys cache locality; see [[Low-Latency and Trading Systems Basics|latency numbers]].

## Related

- [[Algorithmic Patterns for Interviews]] — where each structure gets used
- [[Cpp and Python Under the Hood]] — the language-level layer beneath this one
- [[Low-Latency and Trading Systems Basics]] — why contiguity is money

---
Part of the interview-prep cluster — map: [[SWE Interview Prep]].
