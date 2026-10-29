---
hide:
  - toc
---

# Minimum Transportation Cost (SPFA, Bellman-Ford)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

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
    n = 4

    roads = [
        [1, 2, 2],
        [2, 3, 3]
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

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        SPFA (Shortest Path Faster Algorithm) is a queue-based optimization of the Bellman-Ford algorithm.

        In standard Bellman-Ford, we repeatedly scan **all edges** and try to relax them.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: SPFA

    SPFA (Shortest Path Faster Algorithm) is a queue-based optimization of the Bellman-Ford algorithm.

    In standard Bellman-Ford, we repeatedly scan **all edges** and try to relax them.

    However, if the shortest distance to a node has not changed, there is usually no reason to process all of its outgoing edges again.

    SPFA improves this by using a queue:

    > Only nodes whose shortest distances may cause further updates are added to the queue.
    >

    ## Build the Graph

    Because SPFA needs to process the outgoing edges of a specific node, we use an adjacency list.

    For each directed road:

    ```text
    u --w--> v
    ```

    store:

    ```python
    graph[u].append((w, v))
    ```

    Therefore:

    ```text
    graph[u] = all outgoing edges from node u
    ```

    For example:

    ```text
    1 --2--> 3
    1 --5--> 4
    ```

    is stored as:

    ```python
    graph[1] = [(2, 3), (5, 4)]
    ```

    ## Define `minCost`

    Let:

    ```python
    minCost[i]
    ```

    represent:

    > The minimum transportation cost currently known from city `1` to city `i`.
    >

    Initially, no city is reachable:

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

    ## Initialize the Queue

    We start from city `1`, so initially:

    ```python
    queue = deque([1])
    ```

    We also maintain:

    ```python
    inQueue[i]
    ```

    which represents whether city `i` is currently inside the queue.

    Initially:

    ```python
    inQueue = [False] * (n + 1)
    inQueue[1] = True
    ```

    ## Relax the Outgoing Edges

    Each time we pop a node:

    ```python
    curNode = queue.popleft()
    ```

    it is no longer inside the queue:

    ```python
    inQueue[curNode] = False
    ```

    Then we examine all outgoing edges:

    ```python
    for nextCost, nextNode in graph[curNode]:
    ```

    Suppose we have:

    ```text
    curNode --nextCost--> nextNode
    ```

    The cost of reaching `nextNode` through `curNode` is:

    ```python
    newCost = minCost[curNode] + nextCost
    ```

    If:

    ```python
    newCost < minCost[nextNode]
    ```

    we have found a cheaper path, so update:

    ```python
    minCost[nextNode] = newCost
    ```

    This operation is called **relaxation**.

    ## Why Add `nextNode` Back to the Queue?

    This is the key idea of SPFA.

    Suppose:

    ```text
    1 → 2 → 3
    ```

    and we just found a better path to city `2`.

    Then:

    ```text
    minCost[2] decreases
    ```

    Because city `3` can be reached through city `2`, this new smaller value may also produce:

    ```text
    a smaller minCost[3]
    ```

    Therefore, whenever:

    ```python
    minCost[nextNode]
    ```

    is improved, we need to process the outgoing edges of `nextNode` again.

    So we add it to the queue:

    ```python
    queue.append(nextNode)
    ```

    In other words:

    ```text
    minCost[node] decreases
            ↓
    The outgoing edges of node may now improve other nodes
            ↓
    Put node into the queue
            ↓
    Process it again
    ```

    This is the main difference between SPFA and a normal BFS.

    ## Why Do We Need `inQueue`?

    A node may be relaxed multiple times before it is processed.

    For example:

    ```text
    1 ───────→ 3
     \         ↑
      → 2 ─────┘
    ```

    Suppose city `3` is already waiting inside the queue.

    Before we process it, another path may improve `minCost[3]` again.

    We do **not** need to insert another copy of city `3` into the queue.

    The existing city `3` in the queue will eventually be processed using the newest:

    ```python
    minCost[3]
    ```

    Therefore:

    ```python
    if not inQueue[nextNode]:
        queue.append(nextNode)
        inQueue[nextNode] = True
    ```

    prevents unnecessary duplicate entries.

    ## Why SPFA Can Handle Negative Edges

    Unlike Dijkstra, SPFA does not assume that once a node is processed, its shortest distance is permanently finalized.

    A node can be processed again whenever a shorter path is discovered.

    For example:

    ```text
    1 --5--> 3
    |
    2
    ↓
    2 ---(-10)---> 3
    ```

    We may initially find:

    ```text
    1 → 3 = 5
    ```

    but later discover:

    ```text
    1 → 2 → 3
    = 2 + (-10)
    = -8
    ```

    SPFA can update:

    ```text
    minCost[3]:

    5 → -8
    ```

    and process city `3` again.

    Therefore, negative edge weights are allowed.

    ## When Does SPFA Stop?

    The algorithm continues while:

    ```python
    queue
    ```

    is not empty.

    Eventually, if there is no negative cycle, no more distances can be improved.

    Then:

    ```text
    No relaxation
        ↓
    No new nodes enter the queue
        ↓
    Queue becomes empty
        ↓
    Algorithm terminates
    ```

    At this point:

    ```python
    minCost[i]
    ```

    contains the shortest transportation cost from city `1` to city `i`.

    ## Final Answer

    We only care about the shortest path:

    ```text
    1 → n
    ```

    so the answer is:

    ```python
    minCost[n]
    ```

    If:

    ```python
    minCost[n] == float('inf')
    ```

    city `n` was never reached, so return:

    ```text
    "unconnected"
    ```

    Otherwise:

    ```python
    return minCost[n]
    ```

    ## SPFA Process Summary

    ```text
    Initialize minCost
            ↓
    minCost[1] = 0
            ↓
    Put city 1 into queue
            ↓
    Pop a node
            ↓
    Relax all outgoing edges
            ↓
    Did a neighbor's minCost decrease?
            ↓
           Yes
            ↓
    Is the neighbor already in queue?
          ↙     ↘
        Yes      No
         ↓        ↓
      Do nothing  Add it to queue
            ↓
    Continue until queue is empty
            ↓
    Return minCost[n]
    ```

    ## Bellman-Ford vs SPFA

    ### Bellman-Ford

    Bellman-Ford repeatedly scans all edges:

    ```text
    for n - 1 rounds:
        for every edge:
            try relaxation
    ```

    Even if most nodes have not changed, their related edges may still be checked repeatedly.

    ### SPFA

    SPFA only processes nodes that may produce new relaxations:

    ```text
    distance changes
          ↓
    put node into queue
          ↓
    process its outgoing edges
    ```

    So SPFA can avoid many unnecessary edge checks in practice.

    However, SPFA is **not guaranteed to be faster** than Bellman-Ford.

    Its worst-case time complexity is still:

    ```text
    O(VE)
    ```

    ## Complexity

    Let:

    - `V = n` be the number of cities
    - `E = len(roads)` be the number of roads

    ### Time Complexity

    Worst case:

    ```text
    O(VE)
    ```

    SPFA is often faster in practice because it does not blindly scan every edge in every round, but its worst-case complexity is still the same order as Bellman-Ford.

    ### Space Complexity

    The adjacency list requires:

    ```text
    O(V + E)
    ```

    The `minCost`, `inQueue`, and queue structures require:

    ```text
    O(V)
    ```

    Therefore:

    ```text
    O(V + E)
    ```

    ## Key Takeaways

    ### Bellman-Ford

    ```text
    Repeatedly scan all edges and perform relaxation.
    ```

    ### SPFA

    ```text
    Only process nodes whose shortest distances may cause further relaxation.
    ```

    The most important idea is:

    > **If the shortest distance to a node decreases, put that node into the queue because its outgoing edges may now improve other nodes.**
    >

    Also remember:

    > **SPFA can handle negative edges because a node is allowed to be updated and processed multiple times.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 1, 2026 |

## Python solution

```python
from typing import List
from collections import deque

class Solution:
    def minimumCost(self, n: int, roads: List[List[int]]):
        graph = [[] for _ in range(n + 1)]

        for u, v, w in roads:
            graph[u].append((w, v))

        minCost = [float('inf')] * (n + 1)
        minCost[1] = 0

        queue = deque([1])

        inQueue = [False] * (n + 1)
        inQueue[1] = True

        while queue:
            curNode = queue.popleft()
            inQueue[curNode] = False

            for nextCost, nextNode in graph[curNode]:
                newCost = minCost[curNode] + nextCost

                if newCost < minCost[nextNode]:
                    minCost[nextNode] = newCost

                    if not inQueue[nextNode]:
                        queue.append(nextNode)
                        inQueue[nextNode] = True

        return 'unconnected' if minCost[n] == float('inf') else minCost[n]
```

---

[Back to Day 54 Graph XII](index.md) · [All Algorithm notes](../index.md)
