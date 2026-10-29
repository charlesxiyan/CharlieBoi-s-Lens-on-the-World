---
hide:
  - toc
---

# 743. Network Delay Time (Shortest Path Faster Algorithm, Bellman-Ford)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/network-delay-time/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        SPFA is a queue-based optimization of the **Bellman-Ford algorithm**.

        In standard Bellman-Ford, we repeatedly iterate through **all edges** and perform relaxation.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: SPFA (Shortest Path Faster Algorithm)

    SPFA is a queue-based optimization of the **Bellman-Ford algorithm**.

    In standard Bellman-Ford, we repeatedly iterate through **all edges** and perform relaxation.

    SPFA improves this idea:

    > Only nodes whose shortest distance may affect other nodes need to be processed.
    >

    Therefore, we use a **queue** to store nodes whose outgoing edges need to be relaxed.

    ## Build the Graph

    Since SPFA needs to find all outgoing edges from the current node efficiently, we build an adjacency list.

    ```python
    graph = [[] for _ in range(n + 1)]

    for u, v, w in times:
        graph[u].append((w, v))
    ```

    Each entry stores:

    ```text
    (weight, nextNode)
    ```

    For example:

    ```text
    1 --5--> 2
    ```

    is stored as:

    ```python
    graph[1].append((5, 2))
    ```

    The graph is **directed**, so we only add:

    ```python
    graph[u].append((w, v))
    ```

    and not the reverse edge.

    ## Define `minDist`

    ```python
    minDist = [float('inf')] * (n + 1)
    minDist[k] = 0
    ```

    `minDist[i]` represents:

    > The minimum time required for the signal to travel from node `k` to node `i`.
    >

    Initially:

    ```text
    Starting node k → 0
    Other nodes     → infinity
    ```

    Since nodes are labeled from `1` to `n`, we create an array of size:

    ```text
    n + 1
    ```

    and leave index `0` unused.

    ## Initialize the Queue

    SPFA uses a queue to store nodes whose outgoing edges may need to be relaxed.

    Initially, only the starting node needs to be processed:

    ```python
    queue = deque([k])
    ```

    ## Why Do We Need `inQueue`?

    We also maintain:

    ```python
    inQueue = [False] * (n + 1)
    inQueue[k] = True
    ```

    `inQueue[i]` means:

    > Whether node `i` is currently inside the queue.
    >

    This is different from `visited` in Dijkstra.

    In Dijkstra, a processed node may have its shortest distance finalized.

    In SPFA, a node can be processed multiple times because its shortest distance may be improved again later.

    Therefore, `inQueue` does **not** mean:

    ```text
    This node has already been visited.
    ```

    It only means:

    ```text
    This node is currently waiting inside the queue.
    ```

    ## Pop a Node from the Queue

    ```python
    curNode = queue.popleft()
    inQueue[curNode] = False
    ```

    Once `curNode` is removed from the queue, we mark:

    ```python
    inQueue[curNode] = False
    ```

    because it is no longer inside the queue.

    If its shortest distance is improved again later, it can be added back to the queue.

    ## Relaxation

    For every outgoing edge:

    ```python
    for nextDist, nextNode in graph[curNode]:
    ```

    we calculate:

    ```python
    newDist = minDist[curNode] + nextDist
    ```

    If the new path is shorter:

    ```python
    if newDist < minDist[nextNode]:
    ```

    we update:

    ```python
    minDist[nextNode] = newDist
    ```

    This is exactly the same relaxation operation used in Bellman-Ford:

    ```text
    minDist[v] = min(minDist[v], minDist[u] + weight)
    ```

    ## Why Add the Updated Node to the Queue?

    If:

    ```python
    minDist[nextNode]
    ```

    becomes smaller, then `nextNode` may now be able to improve the distances of its neighbors.

    Therefore, it needs to be processed again.

    ```python
    if not inQueue[nextNode]:
        queue.append(nextNode)
        inQueue[nextNode] = True
    ```

    However, if `nextNode` is already inside the queue, we do not need to add it again.

    Even if its `minDist` is updated multiple times before being popped, when it is eventually processed it will use the latest value:

    ```python
    minDist[nextNode]
    ```

    ## Why Is SPFA an Optimization of Bellman-Ford?

    Standard Bellman-Ford repeatedly scans **all edges**:

    ```python
    for _ in range(n - 1):
        for u, v, w in edges:
            if minDist[u] + w < minDist[v]:
                minDist[v] = minDist[u] + w
    ```

    But consider a node whose `minDist` has not changed.

    Its outgoing edges cannot suddenly produce a better path unless the distance to that node becomes smaller.

    Therefore, scanning all edges every round may perform unnecessary work.

    SPFA instead follows this idea:

    ```text
    A node's minDist is improved
            ↓
    Its outgoing edges may now improve other nodes
            ↓
    Put this node into the queue
            ↓
    Process its outgoing edges
            ↓
    If another node is improved
            ↓
    Put that node into the queue
    ```

    So instead of:

    ```text
    Bellman-Ford:
    Repeatedly scan ALL edges
    ```

    SPFA does:

    ```text
    SPFA:
    Only process nodes whose updated distances
    may cause further relaxation
    ```

    ## SPFA Process

    The overall process can be summarized as:

    ```text
    Initialize minDist
            ↓
    Put start node into queue
            ↓
    Pop curNode
            ↓
    Relax all outgoing edges
            ↓
    Did minDist[nextNode] improve?
           / \
         Yes  No
          |    |
          ↓    |
    Is nextNode already in queue?
           / \
         No   Yes
          |    |
          ↓    |
    Add it    |
    to queue  |
           \  /
            ↓
    Continue until queue is empty
    ```

    When the queue becomes empty, no node can produce any further improvement.

    Therefore, all reachable shortest distances have been found.

    ## Calculate the Answer

    After SPFA finishes:

    ```python
    res = max(minDist[1:])
    ```

    Why do we take the maximum?

    `minDist[i]` represents the earliest time when node `i` receives the signal.

    For example:

    ```text
    Node 1 → 2
    Node 2 → 5
    Node 3 → 3
    Node 4 → 7
    ```

    The signal reaches **all nodes** only when the last node receives it.

    Therefore:

    ```text
    answer = max(minDist)
    ```

    which would be:

    ```text
    7
    ```

    We use:

    ```python
    minDist[1:]
    ```

    because index `0` is unused.

    ## Unreachable Nodes

    If some node cannot be reached from `k`, its distance remains:

    ```python
    float('inf')
    ```

    Therefore:

    ```python
    res = max(minDist[1:])
    ```

    will also be:

    ```text
    inf
    ```

    and we return:

    ```python
    -1
    ```

    Otherwise, return `res`.

    ```python
    return -1 if res == float('inf') else res
    ```

    ## Bellman-Ford vs SPFA

    | Bellman-Ford | SPFA |
    | --- | --- |
    | Iterates through all edges | Processes nodes using a queue |
    | Repeatedly scans every edge | Only processes nodes that may cause updates |
    | No adjacency list required | Usually uses an adjacency list |
    | `O(VE)` worst case | `O(VE)` worst case |
    | Simple and stable | Can be faster in practice on some graphs |

    SPFA does **not** improve the theoretical worst-case complexity of Bellman-Ford.

    Its worst-case time complexity is still:

    ```text
    O(VE)
    ```

    However, in many practical cases, it avoids unnecessary edge relaxations.

    ## SPFA vs Dijkstra

    For LeetCode 743, all edge weights are positive.

    Therefore, both Dijkstra and SPFA can solve the problem.

    | Dijkstra + Heap | SPFA |
    | --- | --- |
    | Uses a priority queue | Uses a normal queue |
    | Processes smallest distance first | Processes updated nodes |
    | Requires non-negative edges | Can handle negative edges |
    | `O(E log V)` | Worst case `O(VE)` |
    | Preferred for LeetCode 743 | Valid, but less efficient theoretically |

    Since LeetCode 743 has only positive edge weights, **Heap Dijkstra is generally the preferred solution**.

    SPFA is useful here mainly for understanding the optimization from Bellman-Ford.

    ## Complexity

    Let:

    ```text
    V = number of nodes
    E = number of edges
    ```

    The worst-case time complexity of SPFA is:

    ```text
    O(VE)
    ```

    The adjacency list requires:

    ```text
    O(V + E)
    ```

    space.

    The queue, `minDist`, and `inQueue` arrays require:

    ```text
    O(V)
    ```

    Therefore:

    ```text
    Time:  O(VE) worst case
    Space: O(V + E)
    ```

    ## Key Takeaway

    The core idea of SPFA is:

    > Only nodes whose shortest distances have been updated need to propagate their new distances to their neighbors.
    >

    The most important template is:

    ```python
    while queue:
        curNode = queue.popleft()
        inQueue[curNode] = False

        for weight, nextNode in graph[curNode]:
            newDist = minDist[curNode] + weight

            if newDist < minDist[nextNode]:
                minDist[nextNode] = newDist

                if not inQueue[nextNode]:
                    queue.append(nextNode)
                    inQueue[nextNode] = True
    ```

    The relationship between the algorithms can be remembered as:

    ```text
    Bellman-Ford
        ↓
    Scan all edges repeatedly

    SPFA
        ↓
    Use a queue
        ↓
    Only process nodes that may cause further relaxation
    ```

    For shortest-path problems:

    ```text
    Positive weights only
    → Dijkstra is usually preferred

    Negative weights allowed
    → Bellman-Ford / SPFA can be used

    Negative cycle exists
    → Shortest path may not be well-defined
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 31, 2026 |

## Python solution

```python
from collections import deque

class Solution:
    def networkDelayTime(
        self,
        times: List[List[int]],
        n: int,
        k: int
    ) -> int:

        queue = deque([k])

        minDist = [float('inf')] * (n + 1)
        minDist[k] = 0

        inQueue = [False] * (n + 1)
        inQueue[k] = True

        graph = [[] for _ in range(n + 1)]

        for u, v, w in times:
            graph[u].append((w, v))

        while queue:
            curNode = queue.popleft()
            inQueue[curNode] = False

            for nextDist, nextNode in graph[curNode]:
                newDist = minDist[curNode] + nextDist

                if newDist < minDist[nextNode]:
                    minDist[nextNode] = newDist

                    if not inQueue[nextNode]:
                        queue.append(nextNode)
                        inQueue[nextNode] = True

        res = max(minDist[1:])

        return -1 if res == float('inf') else res
```

---

[Back to Day 54 Graph XII](index.md) · [All Algorithm notes](../index.md)
