---
hide:
  - toc
---

# 399. Evaluate Division

*Edited by Charles*

**Medium · Python · Weighted Graph · BFS**

## 1. Problem Description

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/evaluate-division/description/).

    Each supplied equation gives a ratio, such as `a / b = 2.0`.
    For each query `start / target`, calculate the implied ratio or return `-1.0`
    if it cannot be determined.

    Ratios are positive and the equations are consistent. Variables not mentioned
    in any equation are unknown. Even `x / x` returns `-1.0` when `x` is unknown.

## 2. Approach and Core Idea

!!! tip "Turn ratios into directed weighted edges"
    For `a / b = w`, add `a → b` with weight `w` and `b → a` with weight `1 / w`.
    A path's product gives the ratio between its endpoints.

Use `defaultdict(list)` to store an adjacency list. Each entry is a pair
`(neighbor, weight)`.

Why multiply the weights? Intermediate variables cancel:

$$
\frac{a}{b}\cdot\frac{b}{c}=\frac{a}{c}.
$$

For BFS, store `(current, product)` in the queue. The invariant is:

$$
\text{product}=\frac{\text{start}}{\text{current}}.
$$

Traversing an edge `current → neighbor` with weight `current / neighbor` gives

$$
\text{new product}
=\frac{\text{start}}{\text{current}}
 \cdot\frac{\text{current}}{\text{neighbor}}
=\frac{\text{start}}{\text{neighbor}}.
$$

**Why it works:** the queue carries a valid derived ratio for each reached
variable. If it reaches the target, that ratio answers the query. If no path
exists, the equations do not connect the two variables, so no ratio can be derived.

## 3. Algorithm Steps

1. Build both directed edges for every equation, including the reciprocal edge.
2. For each query, first check that both variables exist. If either is unknown,
   answer `-1.0`.
3. Initialize a `deque` with `(start, 1.0)` and mark `start` visited.
4. Pop `(current, product)`. If `current == target`, return `product`.
5. For each unvisited neighbor, mark it visited and enqueue
   `(neighbor, product * weight)`.
6. If the queue becomes empty, return `-1.0` for that query.

## 4. Hand Simulation

Suppose:

```text
a / b = 2.0
b / c = 3.0
d / e = 4.0
```

The graph contains:

```text
a --2.0--> b --3.0--> c
b --0.5--> a
c --1/3--> b

d --4.0--> e
e --0.25--> d
```

For the query `a / c`:

| Action | Queue afterward | Meaning |
| --- | --- | --- |
| Initialize | `[(a, 1.0)]` | `a / a = 1.0` |
| Pop a, discover b | `[(b, 2.0)]` | `a / b = 1.0 × 2.0` |
| Pop b, discover c | `[(c, 6.0)]` | `a / c = 2.0 × 3.0` |
| Pop c | Return `6.0` | The target has been reached. |

At b, the edge back to a is skipped because a is already visited.

Other queries illustrate the edge cases:

| Query | Answer | Reason |
| --- | --- | --- |
| `c / a` | $1/6$ | Multiply the reciprocal edges. |
| `a / a` | `1.0` | A known variable divided by itself is one. |
| `a / d` | `-1.0` | Both exist but are in disconnected components. |
| `a / x` | `-1.0` | x is unknown. |
| `x / x` | `-1.0` | An unknown variable is not made valid by matching itself. |

## 5. Important Implementation Details

### Check unknown variables before handling self-queries

If we returned `1.0` merely because `start == target`, we would incorrectly
answer an unknown query such as `x / x`. Membership checks must come first.
Also avoid accessing `graph[start]` before this check: a `defaultdict` access
could create an entry for an unknown name.

### Add reciprocal edges

Knowing `a / b = 2.0` also tells us `b / a = 0.5`. Without the reverse edge,
valid queries in the opposite direction could appear unreachable.

### Reset visited state for every query

A visited set prevents cycling through reciprocal edges. Mark on enqueue so
another route cannot schedule the same variable repeatedly. A new query needs
its own visited set and queue.

### Dijkstra is unnecessary

This is **not a shortest-path optimization problem**. We want a ratio implied
by any connecting path, not a minimum sum or a minimum product. Since the given
equations have no contradictions, different valid paths imply the same ratio
apart from floating-point rounding. BFS can therefore use the first path found.
A priority queue and Dijkstra's distance relaxation add no benefit here.

### Use multiplication, not addition

Weights are ratios, not travel costs. Carry the accumulated product as a float;
small rounding differences are expected for calculations such as `1 / 3`.

## 6. Time and Space Complexity

Let $V$ be the number of known variables, $E$ the number of input equations,
and $Q$ the number of queries. Each equation creates two directed adjacency entries.

| Resource | Complexity | Reason |
| --- | --- | --- |
| Graph construction | $O(E)$ | Add two entries per equation. |
| One query | $O(V+E)$ worst case | BFS may visit the entire reachable component. |
| Total time | $O(E+Q(V+E))$ | Build once, then search independently for each query. |
| Auxiliary space | $O(V+E)$ | Graph storage plus a queue and visited set for one query. |
| Output space | $O(Q)$ | One returned result per query. |

## 7. Complete Python Solution

```python
from collections import defaultdict, deque
from typing import List


class Solution:
    def calcEquation(
        self,
        equations: List[List[str]],
        values: List[float],
        queries: List[List[str]],
    ) -> List[float]:
        graph = defaultdict(list)
        for (numerator, denominator), value in zip(equations, values):
            graph[numerator].append((denominator, value))
            graph[denominator].append((numerator, 1.0 / value))

        def bfs(start: str, target: str) -> float:
            if start not in graph or target not in graph:
                return -1.0

            queue = deque([(start, 1.0)])
            visited = {start}

            while queue:
                current, product = queue.popleft()
                if current == target:
                    return product

                for neighbor, weight in graph[current]:
                    if neighbor not in visited:
                        visited.add(neighbor)
                        queue.append((neighbor, product * weight))

            return -1.0

        return [bfs(start, target) for start, target in queries]
```

## 8. Key Takeaways

- Ratios become edge weights, and reciprocal ratios become reverse edges.
- A BFS state can carry more than a node: here it carries a derived product.
- Known self-queries, unknown variables, and disconnected components differ.
- Weighted graphs do not automatically require Dijkstra; the query's objective matters.

---

[Back to Bonus Graph](index.md) · [All Algorithm notes](../index.md)
