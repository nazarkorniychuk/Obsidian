---
type: concept
topics: [swe-interview]
status: evergreen
created: 2026-09-13
aliases: [leetcode patterns, coding interview patterns, DSA patterns]
---

# Algorithmic Patterns for Interviews

> **Where this sits.** The core of [[SWE Interview Prep]]. Interview problems are ~14 patterns wearing costumes; the skill is *recognition* (cues → pattern) then *recall* (pattern → template). First move on any problem: read the constraints — **input size dictates target complexity**: $n \le 20$ → $O(2^n)$ backtracking/bitmask · $n \le 3000$ → $O(n^2)$ DP · $n \le 10^5$–$10^6$ → $O(n \log n)$ or $O(n)$ · queries + big $n$ → precompute/heap/binary search.

## Recognition table

| cue in the problem | pattern |
|---|---|
| sorted array, pair/triple with property | two pointers |
| contiguous subarray/substring, "longest/shortest window with…" | sliding window |
| subarray *sums* / counts of subarrays | prefix sum + hashmap |
| "minimum X such that feasible" / monotone answer | binary search on the answer |
| next greater/smaller element, histogram areas | monotonic stack |
| k-th largest, top-k, merge k streams, running median | heap(s) |
| overlapping intervals, meeting rooms | sort by start + sweep |
| grid/maze reachability, shortest unweighted path | BFS (DFS for existence/components) |
| ordering with prerequisites, cycle detection in DAG | topological sort |
| dynamic connectivity, "are these merged?" | union-find |
| all combinations/permutations/placements | backtracking |
| "number of ways", "min cost to reach", overlapping subproblems | DP |
| locally best choice provably safe | greedy (+ exchange argument) |
| weighted shortest path | Dijkstra (heap) / Bellman–Ford (neg edges) |

## The patterns

**Two pointers.** Sorted input, shrink from both ends: `while l < r:` move the pointer whose move can only help ($\text{sum} < t \Rightarrow$ `l += 1`). Classics: 2-sum sorted, 3-sum, container with most water, remove duplicates in place. $O(n)$ after sort.

**Sliding window.** Invariant: window always valid (or track best-ever invalid→valid transitions). Grow `r` unconditionally, shrink `l` while invalid; answer updates at each step. Track window state in a counter dict.
```python
l = 0
for r, x in enumerate(a):
    add(x)
    while invalid(): remove(a[l]); l += 1
    best = max(best, r - l + 1)
```
Classics: longest substring without repeats, minimum window substring, max sum subarray of size k, anagram windows.

**Prefix sum + hashmap.** Subarray sum $[i{+}1, j] = P_j - P_i$, so "count subarrays with sum $k$" = for each $j$, look up how many earlier prefixes equal $P_j - k$. Store `seen[prefix] → count`, seed `seen[0] = 1`. Classics: subarray sum equals k, longest subarray with equal 0s/1s, product-under-k variant with logs. $O(n)$.

**Binary search on the answer.** When the *answer itself* is monotone-feasible ("can we do it with capacity $c$? then also with $c+1$"), binary search over answers, greedy feasibility check inside. Classics: Koko bananas, split array largest sum, ship packages in D days, min-max distance placements.
```python
lo, hi = min_ans, max_ans
while lo < hi:
    mid = (lo + hi) // 2
    if feasible(mid): hi = mid
    else: lo = mid + 1
```

**Monotonic stack.** For next-greater/smaller queries in $O(n)$: keep stack of indices with monotone values; pop while the invariant breaks — the popper is the popped elements' answer. Classics: daily temperatures, largest rectangle in histogram (stack of increasing heights), trapping rain water, stock span.

**Heaps.** Top-k = min-heap of size k ($O(n \log k)$); k-way merge = heap of heads; running median = max-heap lower half + min-heap upper half, rebalance to sizes within 1. Python: `heapq` is a min-heap — negate for max. Classics: kth largest, merge k sorted lists, median from stream, task scheduler.

**Intervals.** Sort by start. Merge: extend current while `next.start <= cur.end`. Min meeting rooms = sweep line (+1 at starts, −1 at ends, max prefix) or min-heap of end times. Non-overlapping-max = sort by *end*, greedy take.

**BFS / DFS.** BFS = shortest path in unweighted graphs, level by level (queue + visited-on-enqueue); multi-source BFS = seed queue with all sources (rotting oranges, walls-and-gates); 0-1 BFS = deque. DFS = existence, components, islands (grid flood fill). Mark visited **when enqueuing**, not dequeuing (else duplicates). Classics: word ladder, number of islands, shortest path in binary matrix.

**Topological sort.** Kahn's: indegree count, queue of zeros, pop-and-decrement; if processed < V, there's a cycle. Classics: course schedule I/II, alien dictionary, build orders.

**Union-find.** Near-$O(1)$ dynamic connectivity: parent array + path compression + union by size.
```python
def find(x):
    while p[x] != x: p[x] = p[p[x]]; x = p[x]
    return x
```
Classics: number of provinces, accounts merge, redundant connection, Kruskal MST.

**Backtracking.** Choose → recurse → unchoose; prune early. Subsets/permutations/combination-sum templates differ only in the loop start index and reuse rule. Dedup on sorted input: skip `a[i] == a[i-1]` at the same depth. Classics: N-queens, word search, palindrome partitioning. Budget: $n \le 20$.

**Dynamic programming.** The four questions: *state* (what suffices to describe a subproblem), *transition*, *base*, *order*. The families to know cold:
- **1D linear**: climb stairs, house robber ($dp_i = \max(dp_{i-1}, dp_{i-2} + a_i)$), max subarray (Kadane)
- **Knapsack**: 0/1 (iterate capacity *downward*), unbounded (upward), subset-sum/partition
- **LIS**: $O(n \log n)$ via patience (tails array + binary search)
- **Grid**: unique paths, min path sum, edit distance / LCS ($dp_{ij}$ from three neighbors)
- **Interval**: burst balloons, matrix chain — iterate by interval length
- **Bitmask**: TSP-style, $dp[\text{mask}][i]$, $n \le 20$
Memoized recursion first (correct, fast to write), convert to tabulation only if pressed.

**Greedy + exchange argument.** Only claim greedy when you can say *why*: "swapping any two adjacent choices in my order never improves the result." Classics: jump game, gas station, non-overlapping intervals, Huffman-style merges (heap).

**Dijkstra.** Non-negative weights: heap of `(dist, node)`, skip stale pops (`if d > dist[u]: continue`). $O(E \log V)$. Negative edges → Bellman–Ford; all pairs small graph → Floyd–Warshall. Classics: network delay, cheapest flights within k stops (that one is Bellman–Ford/BFS-layered, *not* plain Dijkstra).

## The follow-up game (Citadel-specific)

Solved it? The interviewer will move the goalposts — have the ladder ready: brute force → hashmap/sort → heap/binary search → linear/monotonic → streaming ($O(1)$ memory) → concurrent. For every problem you practice, write one sentence: *"if input were 100× larger / arrived as a stream / had to answer queries online, I would…"* That sentence is the superday.

## Related

- [[Data Structures Under the Hood]] — what each structure costs and why
- [[Probability and Brainteasers]] — the randomized-algorithms overlap (sampling, shuffle)
- [[SWE Interview Prep]] — the map and drill plan

---
Part of the interview-prep cluster — map: [[SWE Interview Prep]].
