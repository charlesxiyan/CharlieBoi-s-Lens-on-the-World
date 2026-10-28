---
hide:
  - toc
---

# 802. Find Eventual Safe States (Topological Sort)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-eventual-safe-states/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Use **Reverse Graph + Kahn's Algorithm**.

    A node is an **eventual safe node** if every possible path starting from it eventually reaches a terminal node and never enters a cycle.

## Python solution

```python
from collections import deque

class Solution:
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        l = len(graph)
        revGraph = [[] for _ in range(l)]
        outdegree = [0] * l

        for i in range(l):
            outdegree[i] = len(graph[i])

            for nextNode in graph[i]:
                revGraph[nextNode].append(i)

        queue = deque()

        for i in range(l):
            if outdegree[i] == 0:
                queue.append(i)

        res = []

        while queue:
            node = queue.popleft()
            res.append(node)

            for preNode in revGraph[node]:
                outdegree[preNode] -= 1

                if outdegree[preNode] == 0:
                    queue.append(preNode)

        return sorted(res)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Reverse Graph + Kahn's Algorithm**.

    A node is an **eventual safe node** if every possible path starting from it eventually reaches a terminal node and never enters a cycle.

    The key observation is:

    > A node with `outdegree == 0` is a terminal node, so it is definitely safe.
    >

    We can start from all terminal nodes and propagate safety backward.

    ## Why Reverse the Graph?

    Suppose the original graph has:

    ```text
    u → v
    ```

    If `v` is already known to be safe, we want to know:

    ```text
    Which nodes point to v?
    ```

    So we reverse every edge:

    ```text
    u → v
    ```

    into:

    ```text
    v → u
    ```

    In the code:

    ```python
    revGraph[nextNode].append(i)
    ```

    Now:

    ```python
    revGraph[node]
    ```

    stores all predecessor nodes that originally point to `node`.

    ## Outdegree

    Define:

    ```python
    outdegree[i] = len(graph[i])
    ```

    `outdegree[i]` represents how many outgoing edges node `i` currently has that have not yet been confirmed safe.

    If:

    ```python
    outdegree[i] == 0
    ```

    then node `i` is a terminal node and can be added to the queue immediately.

    ## Initialize the Queue

    ```python
    for i in range(l):
        if outdegree[i] == 0:
            queue.append(i)
    ```

    All terminal nodes are initially safe.

    ## BFS / Kahn's Algorithm

    Each time we remove a safe node from the queue:

    ```python
    node = queue.popleft()
    res.append(node)
    ```

    Then check all nodes that originally point to it:

    ```python
    for preNode in revGraph[node]:
    ```

    Since `node` is safe, one outgoing edge of `preNode` has now been confirmed safe:

    ```python
    outdegree[preNode] -= 1
    ```

    If:

    ```python
    outdegree[preNode] == 0
    ```

    then all outgoing edges of `preNode` lead to safe nodes.

    Therefore, `preNode` is also safe:

    ```python
    queue.append(preNode)
    ```

    ## Why Does This Exclude Cycles?

    Nodes inside a cycle always have at least one outgoing edge that stays inside the cycle.

    For example:

    ```text
    0 → 1
    ↑   ↓
    └───┘
    ```

    Neither node can eventually reduce its `outdegree` to `0`.

    Therefore, cycle nodes are never added to the queue and are automatically excluded from the result.

    ## Key Insight

    Think of:

    ```python
    outdegree[node]
    ```

    as:

    > How many outgoing paths are still not proven safe?
    >

    Whenever one destination is confirmed safe:

    ```python
    outdegree[preNode] -= 1
    ```

    When this value becomes:

    ```python
    0
    ```

    all outgoing paths from that node are safe, so the node itself is also safe.

    ## Why Use `revGraph` Instead of `graph`?

    When a node becomes safe, we need to move backward and update its predecessors.

    Therefore, we use:

    ```python
    for preNode in revGraph[node]:
    ```

    instead of:

    ```python
    for nextNode in graph[node]:
    ```

    The original graph tells us:

    ```text
    node → nextNode
    ```

    but we need to find:

    ```text
    preNode → node
    ```

    which is stored in the reversed graph.

    ## Why `sorted(res)`?

    Kahn's Algorithm processes nodes according to queue order, so the safe nodes are not guaranteed to be collected in ascending order.

    The problem requires the result in sorted order:

    ```python
    return sorted(res)
    ```

    Also remember:

    ```python
    res.sort()
    ```

    sorts the list in place and returns `None`.

    So we should not write:

    ```python
    return res.sort()
    ```

    ## Complexity

    Let:

    - `V` = number of nodes
    - `E` = number of edges

    Building the reverse graph takes:

    ```text
    O(V + E)
    ```

    BFS processes each node and edge at most once:

    ```text
    O(V + E)
    ```

    Sorting the result takes:

    ```text
    O(V log V)
    ```

    Overall:

    ```text
    Time: O(V + E + V log V)
    Space: O(V + E)
    ```

    ## Code



    > **Memory:** Start from terminal nodes, reverse the graph, and propagate safety backward.
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 27, 2026 |

---

[Back to Day 50 Graph VIII](index.md) · [All Algorithm notes](../index.md)
