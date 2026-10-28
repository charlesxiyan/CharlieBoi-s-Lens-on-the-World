---
hide:
  - toc
---

# Minimum Transportation Cost (Negative Cycles, Bellman-Ford)

*Edited by Charles*

**Medium · Python · Graph**

!!! tip "Core idea"

    Bellman-Ford is a shortest-path algorithm that can handle:

    - Positive edge weights
    - Zero edge weights
    - Negative edge weights

## Python solution

```python
from typing import List

class Solution:
    def minimumCost(self, n: int, roads: List[List[int]]):
        minCost = [float('inf')] * (n + 1)
        minCost[1] = 0

        for _ in range(n - 1):
            for u, v, w in roads:
                if (
                    minCost[u] != float('inf')
                    and minCost[v] > minCost[u] + w
                ):
                    minCost[v] = minCost[u] + w

        # One more iteration to detect a negative cycle
        for u, v, w in roads:
            if (
                minCost[u] != float('inf')
                and minCost[v] > minCost[u] + w
            ):
                return 'circle'

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

    However, the road network may contain **negative-weight cycles**.

    A negative-weight cycle is a cycle whose total edge weight is negative. By repeatedly traveling through such a cycle, the total transportation cost can theoretically be reduced indefinitely.

    Your task is to find the **minimum transportation cost from city `1` to city `n`**, while also detecting whether a negative-weight cycle exists.

    It is also possible that city `n` cannot be reached from city `1`.

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

    Return the minimum transportation cost from city `1` to city `n` if a valid minimum cost exists.

    If the minimum cost is negative, it represents the amount of profit generated after government subsidies.

    If a negative-weight cycle is detected, return:

    ```text
    "circle"
    ```

    If city `n` cannot be reached from city `1`, return:

    ```text
    "unconnected"
    ```

    ## Example 1

    ### Input

    ```python
    n = 4

    roads = [
        [1, 2, -1],
        [2, 3, 1],
        [3, 1, -1],
        [3, 4, 1]
    ]
    ```

    ### Output

    ```text
    "circle"
    ```

    ## Example 2

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

    ## Example 3

    ### Input

    ```python
    n = 4

    roads = [
        [1, 2, 2],
        [2, 3, -1]
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
    - The graph may contain negative-weight cycles.
    - Multiple paths may exist between two cities.
    - City `n` may be unreachable from city `1`.

    ## Approach: Bellman-Ford + Negative Cycle Detection

    Bellman-Ford is a shortest-path algorithm that can handle:

    - Positive edge weights
    - Zero edge weights
    - Negative edge weights

    Unlike Dijkstra, Bellman-Ford does not require all edge weights to be non-negative.

    The core operation is **relaxation**.

    For every edge:

    ```text
    u --w--> v
    ```

    if:

    ```python
    minCost[u] + w < minCost[v]
    ```

    then going from city `1` to `u`, followed by the edge `u → v`, gives us a cheaper path to `v`.

    Therefore we update:

    ```python
    minCost[v] = minCost[u] + w
    ```

    ## Define `minCost`

    Let:

    ```python
    minCost[i]
    ```

    represent:

    > The minimum transportation cost currently known from city `1` to city `i`.
    >

    Initially:

    ```python
    minCost = [float('inf')] * (n + 1)
    ```

    Since the cost of traveling from city `1` to itself is `0`:

    ```python
    minCost[1] = 0
    ```

    So initially:

    ```text
    City:       1    2    3    ...    n
    minCost:    0    ∞    ∞    ...    ∞
    ```

    ## Relax Every Edge

    Suppose we have an edge:

    ```text
    u --w--> v
    ```

    If city `u` is already reachable from city `1`, then the cost of reaching `v` through `u` is:

    ```text
    minCost[u] + w
    ```

    If this is smaller than the current:

    ```python
    minCost[v]
    ```

    we update it:

    ```python
    if (
        minCost[u] != float('inf')
        and minCost[v] > minCost[u] + w
    ):
        minCost[v] = minCost[u] + w
    ```

    The condition:

    ```python
    minCost[u] != float('inf')
    ```

    is important because we can only use the edge:

    ```text
    u → v
    ```

    if city `u` is reachable from city `1`.

    ## Why Relax `n - 1` Times?

    There are `n` cities.

    If there is no negative cycle, a shortest path does not need to repeatedly visit the same node.

    Therefore, a shortest simple path can contain at most:

    ```text
    n nodes
    ```

    which means it can contain at most:

    ```text
    n - 1 edges
    ```

    For example:

    ```text
    1 → 3 → 5 → 2 → 6
    ```

    If there are 5 nodes on the path, there are 4 edges.

    Therefore Bellman-Ford performs:

    ```python
    for _ in range(n - 1):
    ```

    iterations.

    During each iteration, we scan every edge and try to relax it.

    ## Important Note About the `n - 1` Iterations

    In this implementation, `minCost` is updated **in place**.

    This means a newly updated distance may immediately be used by another edge later in the same iteration.

    For example, if the edge order is:

    ```text
    1 → 2
    2 → 3
    3 → 4
    ```

    then during just one scan:

    ```text
    1 → 2
        ↓
    update 2

    2 → 3
        ↓
    use the new value of 2
        ↓
    update 3

    3 → 4
        ↓
    use the new value of 3
        ↓
    update 4
    ```

    So one iteration may propagate information across multiple edges.

    Therefore:

    > `n - 1` is a safe upper bound on the number of full edge scans required.
    >

    It does **not** necessarily mean that exactly one additional edge is processed per iteration.

    ## Why Can Bellman-Ford Detect Negative Cycles?

    This is the most important part of this problem.

    If there is no negative cycle, after enough relaxations, all shortest distances eventually become stable.

    After performing:

    ```text
    n - 1
    ```

    full scans of all edges, there should be no edge that can produce a smaller distance.

    In other words, normally:

    ```python
    minCost[u] + w >= minCost[v]
    ```

    for every reachable edge.

    But suppose after `n - 1` iterations we scan all edges **one more time** and still find:

    ```python
    minCost[u] + w < minCost[v]
    ```

    Then the shortest distance is still decreasing.

    That means some path is able to repeatedly reduce its total cost.

    This can only happen because there is a reachable **negative-weight cycle**.

    ## Example of a Negative Cycle

    Consider:

    ```text
    1 --(-1)--> 2
    2 --( 1)--> 3
    3 --(-1)--> 1
    ```

    The total cost of the cycle is:

    ```text
    -1 + 1 - 1 = -1
    ```

    Every time we travel around this cycle, the total cost decreases by `1`.

    For example:

    ```text
    Start:

    cost = 0

    After one cycle:

    cost = -1

    After two cycles:

    cost = -2

    After three cycles:

    cost = -3

    ...
    ```

    Therefore the minimum cost has no lower bound.

    It can continue decreasing forever.

    ## Detect the Negative Cycle

    After the normal `n - 1` Bellman-Ford iterations, scan every edge one additional time:

    ```python
    for u, v, w in roads:
    ```

    If any edge can still be relaxed:

    ```python
    if (
        minCost[u] != float('inf')
        and minCost[v] > minCost[u] + w
    ):
    ```

    then return:

    ```python
    "circle"
    ```

    The structure is:

    ```text
    Relax all edges n - 1 times
            ↓
    Scan all edges one more time
            ↓
    Can any edge still be relaxed?
           ↙            ↘
         Yes             No
          ↓               ↓
    negative cycle     continue
          ↓
      "circle"
    ```

    ## Why Check `minCost[u] != inf` When Detecting a Cycle?

    Consider a negative cycle that exists somewhere else in the graph:

    ```text
    1 → 2 → n

    5 → 6
    ↑   ↓
    └── 7
    ```

    Suppose:

    ```text
    5 → 6 → 7 → 5
    ```

    is a negative cycle, but city `1` cannot reach cities `5`, `6`, or `7`.

    Then this negative cycle has no effect on paths starting from city `1`.

    Because:

    ```python
    minCost[5] == inf
    minCost[6] == inf
    minCost[7] == inf
    ```

    the condition:

    ```python
    minCost[u] != float('inf')
    ```

    prevents these unreachable edges from being considered.

    Therefore, this implementation detects:

    > Negative cycles that are reachable from city `1`.
    >

    ## Final Answer

    If no reachable negative cycle is found, check:

    ```python
    minCost[n]
    ```

    If:

    ```python
    minCost[n] == float('inf')
    ```

    then city `n` cannot be reached from city `1`.

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
    Try relaxation
            ↓
    After n - 1 iterations
            ↓
    Scan every edge one more time
            ↓
    Can any edge still be relaxed?
          ↙               ↘
        Yes                No
         ↓                  ↓
    "circle"         Check minCost[n]
                      ↙           ↘
                    inf         finite
                     ↓             ↓
             "unconnected"    minCost[n]
    ```

    ## Bellman-Ford vs Dijkstra

    ### Dijkstra

    Dijkstra requires:

    ```text
    All edge weights >= 0
    ```

    Once a node is finalized, its shortest distance will never change again.

    ### Bellman-Ford

    Bellman-Ford allows:

    ```text
    Negative edge weights
    ```

    A node's shortest distance may decrease multiple times.

    Therefore Bellman-Ford repeatedly relaxes edges until the distances become stable.

    ## Bellman-Ford vs SPFA

    Bellman-Ford scans:

    ```text
    ALL edges
    ```

    during every iteration.

    Its basic structure is:

    ```text
    repeat n - 1 times:
        scan every edge
    ```

    SPFA instead uses a queue and mainly processes nodes whose distances have changed:

    ```text
    distance decreases
           ↓
    put node into queue
           ↓
    process its outgoing edges
    ```

    So SPFA can avoid some unnecessary edge scans in practice.

    However, both algorithms have the same worst-case time complexity:

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

    Bellman-Ford performs up to:

    ```text
    V - 1
    ```

    iterations.

    Each iteration scans:

    ```text
    E
    ```

    edges.

    The additional negative-cycle check also scans `E` edges.

    Therefore:

    ```text
    O(VE)
    ```

    ### Space Complexity

    We only need:

    ```python
    minCost
    ```

    which contains `V` values.

    Therefore:

    ```text
    O(V)
    ```

    No adjacency list is required because Bellman-Ford can directly iterate over the edge list.

    ## Key Takeaways

    ### Relaxation

    For an edge:

    ```text
    u --w--> v
    ```

    try:

    ```python
    minCost[v] = min(
        minCost[v],
        minCost[u] + w
    )
    ```

    ### Why `n - 1`?

    A shortest simple path in a graph with `n` nodes contains at most:

    ```text
    n - 1 edges
    ```

    ### Negative Cycle Detection

    The most important rule to remember is:

    > **Relax edges for `n - 1` iterations, then scan all edges one more time.**
    >

    If an edge can still be relaxed:

    ```text
    distance can still decrease
            ↓
    negative cycle exists
    ```

    A useful memory shortcut is:

    ```text
    n - 1 passes → shortest paths
    1 extra pass → negative cycle detection
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 1, 2026 |

---

[Back to Day 54 Graph XII](index.md) · [All Algorithm notes](../index.md)
