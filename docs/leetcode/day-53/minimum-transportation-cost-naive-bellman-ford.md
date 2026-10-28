---
hide:
  - toc
---

# Minimum Transportation Cost (Naive Bellman-Ford)

*Edited by Charles*

**Medium · Python · Graph**

!!! tip "Core idea"

    Bellman-Ford is a shortest-path algorithm that can handle graphs containing **negative edge weights**.

    Unlike Dijkstra, Bellman-Ford does not require all edge weights to be non-negative.

## Python solution

```python
from typing import List

class Solution:
    def minimumCost(self, n: int, roads: List[List[int]]):
        minCost = [float('inf')] * (n + 1)
        minCost[1] = 0

        for _ in range(n - 1):
            updated = False
            for u, v, w in roads:
                if (
                    minCost[u] != float('inf')
                    and minCost[v] > minCost[u] + w
                ):
                    minCost[v] = minCost[u] + w
                    updated = True

            # 如果已经不往更小的值更新了，那就是找到最小路径了，退出迭代
            if not updated:
                break

        return 'unconnected' if minCost[n] == float('inf') else minCost[n]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem Description

    A country wants to promote economic exchange between cities by providing subsidies for cargo transportation.

    There are `n` cities numbered from `1` to `n`. The cities are connected by a network of **directed roads**, meaning that a road from city `u` to city `v` cannot necessarily be traveled in the opposite direction.

    Each road has a transportation cost and a government subsidy. The final weight of a road is calculated as:

    > **Weight = Transportation Cost - Government Subsidy**
    >

    A positive weight means that transportation still costs money after applying the government subsidy.

    A negative weight means that the government subsidy exceeds the transportation cost, so traveling along that road actually generates a profit.

    Your task is to find the **minimum transportation cost from city `1` to city `n`** after considering all government subsidies.

    If the minimum transportation cost is negative, it means that transportation generates a net profit.

    It is possible that city `n` cannot be reached from city `1`.

    The road network is guaranteed to contain **no negative-weight cycles**.

    ## Function Signature

    ```python
    class Solution:
        def minimumCost(self, n: int, roads: List[List[int]]):
    ```

    ## Parameters

    ### `n`

    The number of cities.

    Cities are numbered:

    ```text
    1, 2, 3, ..., n
    ```

    City `1` is the starting city.

    City `n` is the destination.

    ### `roads`

    `roads[i] = [u, v, w]` represents a directed road from city `u` to city `v` with weight `w`.

    ```text
    u --w--> v
    ```

    The weight `w` may be:

    - Positive
    - Zero
    - Negative

    ## Return Value

    Return the minimum transportation cost from city `1` to city `n`.

    If the minimum cost is negative, it represents the amount of profit generated after government subsidies.

    If city `n` cannot be reached from city `1`, return:

    ```text
    "unconnected"
    ```

    ## Example 1

    ### Input

    ```python
    n = 6

    roads = [
        [5, 6, -2],
        [1, 2, 1],
        [5, 3, 1],
        [2, 5, 2],
        [2, 4, -3],
        [4, 6, 4],
        [1, 3, 5]
    ]
    ```

    ### Output

    ```text
    1
    ```

    ## Example 2

    ### Input

    ```python
    n = 4

    roads = [
        [1, 2, 2],
        [2, 3, -4],
        [3, 4, 1],
        [1, 4, 5]
    ]
    ```

    ### Output

    ```text
    -1
    ```

    ## Example 3

    ### Input

    ```python
    n = 5

    roads = [
        [1, 2, 3],
        [2, 3, -2],
        [3, 4, 1]
    ]
    ```

    ### Output

    ```text
    "unconnected"
    ```

    ## Constraints

    - `2 <= n`
    - `1 <= len(roads)`
    - `roads[i] = [u, v, w]`
    - `1 <= u, v <= n`
    - The graph is directed.
    - Edge weights may be positive, zero, or negative.
    - The graph does **not** contain negative-weight cycles.
    - Multiple paths may exist between two cities.
    - City `n` may be unreachable from city `1`.

    ## Approach: Naive Bellman-Ford

    Bellman-Ford is a shortest-path algorithm that can handle graphs containing **negative edge weights**.

    Unlike Dijkstra, Bellman-Ford does not require all edge weights to be non-negative.

    Its core idea is simple:

    > Repeatedly scan every edge and try to improve the shortest distance to each node.
    >

    This operation is called **relaxation**.

    ## Define `minCost`

    Let:

    ```python
    minCost[i]
    ```

    represent:

    > The minimum transportation cost currently known from city `1` to city `i`.
    >

    Initially, all cities are considered unreachable:

    ```python
    minCost = [float('inf')] * (n + 1)
    ```

    The starting city has cost `0`:

    ```python
    minCost[1] = 0
    ```

    So initially:

    ```text
    City:       1    2    3    ...    n
    minCost:    0    ∞    ∞    ...    ∞
    ```

    ## Relax an Edge

    Suppose there is a directed edge:

    ```text
    u --w--> v
    ```

    If we already know the shortest cost to reach `u`, then traveling through this edge gives a possible cost to reach `v`:

    ```text
    minCost[u] + w
    ```

    If:

    ```python
    minCost[u] + w < minCost[v]
    ```

    then we found a cheaper path to `v`.

    So we update:

    ```python
    minCost[v] = minCost[u] + w
    ```

    The full condition is:

    ```python
    if (
        minCost[u] != float('inf')
        and minCost[v] > minCost[u] + w
    ):
        minCost[v] = minCost[u] + w
    ```

    ## Why Check `minCost[u] != inf`?

    Suppose city `u` has never been reached from city `1`.

    Then:

    ```python
    minCost[u] == float('inf')
    ```

    In that case, we cannot use the edge:

    ```text
    u → v
    ```

    because there is currently no valid path from city `1` to `u`.

    Therefore we only perform relaxation when:

    ```python
    minCost[u] != float('inf')
    ```

    ## Why Relax All Edges `n - 1` Times?

    There are `n` cities.

    If the graph contains no negative-weight cycle, a shortest path never needs to repeatedly visit the same node.

    Therefore, a shortest simple path can contain at most:

    ```text
    n nodes
    ```

    which means at most:

    ```text
    n - 1 edges
    ```

    For example:

    ```text
    1 → 3 → 5 → 2 → 6
    ```

    contains 5 nodes and 4 edges.

    Therefore Bellman-Ford performs at most:

    ```python
    for _ in range(n - 1):
    ```

    full scans of all edges.

    After these scans, all shortest distances are guaranteed to be found if there is no reachable negative cycle.

    ## Why Do We Scan Every Edge?

    Bellman-Ford works directly on the edge list.

    For every iteration:

    ```python
    for u, v, w in roads:
    ```

    we check whether the edge:

    ```text
    u → v
    ```

    can improve:

    ```python
    minCost[v]
    ```

    Unlike Dijkstra, Bellman-Ford does not permanently finalize a node.

    A node's distance may be updated multiple times.

    This is why Bellman-Ford can handle negative edges.

    ## Example of Relaxation

    Consider:

    ```text
    1 --4--> 2
    1 --10-> 3
    2 --(-6)--> 3
    ```

    Initially:

    ```text
    minCost[1] = 0
    minCost[2] = ∞
    minCost[3] = ∞
    ```

    Using:

    ```text
    1 → 2
    ```

    we get:

    ```text
    minCost[2] = 4
    ```

    Using:

    ```text
    1 → 3
    ```

    we get:

    ```text
    minCost[3] = 10
    ```

    Then using:

    ```text
    2 → 3
    ```

    we get:

    ```text
    4 + (-6) = -2
    ```

    Since:

    ```text
    -2 < 10
    ```

    we update:

    ```text
    minCost[3] = -2
    ```

    So the shortest path is:

    ```text
    1 → 2 → 3
    ```

    with total cost:

    ```text
    4 + (-6) = -2
    ```

    ## In-Place Updates

    In this implementation, `minCost` is updated directly during each iteration.

    For example, suppose the edges appear in this order:

    ```text
    1 → 2
    2 → 3
    3 → 4
    ```

    During one full scan:

    ```text
    1 → 2
        ↓
    update minCost[2]

    2 → 3
        ↓
    use the newly updated minCost[2]
        ↓
    update minCost[3]

    3 → 4
        ↓
    use the newly updated minCost[3]
        ↓
    update minCost[4]
    ```

    Therefore, one iteration may propagate shortest-path information across multiple edges.

    Because of this:

    > `n - 1` iterations are a safe upper bound, but Bellman-Ford may converge earlier depending on the edge order.
    >

    ## Why Can Bellman-Ford Handle Negative Edges?

    Dijkstra relies on the assumption that once the currently closest node is selected, its shortest distance will never decrease again.

    This is not true when negative edges exist.

    For example:

    ```text
    1 --5--> 3
    |
    2
    ↓
    2 --(-10)--> 3
    ```

    Initially we may find:

    ```text
    1 → 3 = 5
    ```

    but later:

    ```text
    1 → 2 → 3
    = 2 + (-10)
    = -8
    ```

    So the distance to city `3` changes:

    ```text
    5 → -8
    ```

    Bellman-Ford allows this because nodes can be relaxed repeatedly.

    ## Why Is There No Negative Cycle Detection Here?

    This problem guarantees:

    ```text
    The graph does not contain negative-weight cycles.
    ```

    Therefore we only need to calculate the shortest paths.

    We do not need the additional Bellman-Ford pass used for negative-cycle detection.

    If negative cycles were allowed, we would perform one additional scan after the `n - 1` iterations.

    If any edge could still be relaxed, that would indicate a reachable negative cycle.

    ## Final Answer

    We only care about the shortest path:

    ```text
    1 → n
    ```

    So after Bellman-Ford finishes, check:

    ```python
    minCost[n]
    ```

    If:

    ```python
    minCost[n] == float('inf')
    ```

    then city `n` is unreachable from city `1`.

    Return:

    ```text
    "unconnected"
    ```

    Otherwise:

    ```python
    return minCost[n]
    ```

    ## Bellman-Ford Process Summary

    ```text
    Initialize minCost
            ↓
    minCost[1] = 0
            ↓
    Repeat n - 1 times
            ↓
    Scan every edge
            ↓
    For each edge u → v
            ↓
    Can minCost[u] + w improve minCost[v]?
            ↓
           Yes
            ↓
    Update minCost[v]
            ↓
    Continue
            ↓
    Check minCost[n]
          ↙           ↘
        inf          finite
         ↓              ↓
    "unconnected"   minCost[n]
    ```

    ## Bellman-Ford vs Dijkstra

    ### Dijkstra

    Dijkstra requires:

    ```text
    All edge weights >= 0
    ```

    Once a node is finalized, its shortest distance will never change.

    ### Bellman-Ford

    Bellman-Ford allows:

    ```text
    Negative edge weights
    ```

    A node's shortest distance may change multiple times.

    Bellman-Ford repeatedly scans edges and performs relaxation until shortest distances are obtained.

    ## Bellman-Ford vs SPFA

    ### Naive Bellman-Ford

    Repeatedly scans **all edges**:

    ```text
    repeat n - 1 times:
        scan every edge
    ```

    ### SPFA

    Uses a queue and mainly processes nodes whose shortest distances have changed:

    ```text
    distance decreases
           ↓
    put node into queue
           ↓
    process its outgoing edges
    ```

    SPFA may avoid unnecessary edge scans in practice.

    However, both have worst-case complexity:

    ```text
    O(VE)
    ```

    ## Complexity

    Let:

    ```text
    V = n
    E = len(roads)
    ```

    ### Time Complexity

    Bellman-Ford performs at most:

    ```text
    V - 1
    ```

    iterations.

    Each iteration scans:

    ```text
    E
    ```

    edges.

    Therefore:

    ```text
    O(VE)
    ```

    ### Space Complexity

    We only need:

    ```python
    minCost
    ```

    which stores one value for each city.

    Therefore:

    ```text
    O(V)
    ```

    Bellman-Ford does not require an adjacency list because it directly scans the edge list.

    ## Optional Optimization

    We can use an `updated` flag.

    If one complete iteration does not update any distance, then all shortest distances are already stable.

    So we can stop early:

    ```python
    for _ in range(n - 1):
        updated = False

        for u, v, w in roads:
            if (
                minCost[u] != float('inf')
                and minCost[v] > minCost[u] + w
            ):
                minCost[v] = minCost[u] + w
                updated = True

        if not updated:
            break
    ```

    This does not change the worst-case complexity, but it may reduce unnecessary iterations.

    ## Key Takeaways

    The core of Bellman-Ford is **relaxation**:

    ```text
    u --w--> v
    ```

    Try:

    ```python
    minCost[v] = min(
        minCost[v],
        minCost[u] + w
    )
    ```

    The most important ideas are:

    ```text
    Negative edges are allowed.

    Scan all edges repeatedly.

    At most n - 1 full scans are required.

    A node's shortest distance may be updated multiple times.
    ```

    A useful memory shortcut is:

    ```text
    Dijkstra
    → repeatedly choose the closest node

    Bellman-Ford
    → repeatedly relax every edge
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 1, 2026 |

---

[Back to Day 53 Graph XI](index.md) · [All LeetCode notes](../index.md)
