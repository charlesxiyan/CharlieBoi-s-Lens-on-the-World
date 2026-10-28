---
hide:
  - toc
---

# 743. Network Delay Time (Dijkstra + minHeap)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/network-delay-time/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    This is a **single-source shortest path** problem.

    We need to find the shortest distance from the starting node `k` to every other node. Since all edge weights are positive, we can use **Dijkstra's algorithm**.

## Python solution

```python
import heapq

class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        graph = [[] for _ in range(n + 1)]

        for u, v, w in times:
            graph[u].append((w, v))

        minDist = [float('inf')] * (n + 1)
        minDist[k] = 0

        minHeap = [(0, k)]

        while minHeap:
            curDist, curNode = heapq.heappop(minHeap)

            if curDist > minDist[curNode]:
                continue

            for nextDist, nextNode in graph[curNode]:
                newDist = curDist + nextDist

                if newDist < minDist[nextNode]:
                    minDist[nextNode] = newDist
                    heapq.heappush(minHeap, (newDist, nextNode))

        res = max(minDist[1:])

        return res if res != float('inf') else -1
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: Dijkstra + Min Heap

    This is a **single-source shortest path** problem.

    We need to find the shortest distance from the starting node `k` to every other node. Since all edge weights are positive, we can use **Dijkstra's algorithm**.

    A **min heap** is used to efficiently select the node with the smallest current distance.

    ## Build the Graph

    Use an adjacency list:

    ```python
    graph = [[] for _ in range(n + 1)]

    for u, v, w in times:
        graph[u].append((w, v))
    ```

    Each edge is stored as:

    ```text
    (weight, nextNode)
    ```

    For example:

    ```text
    2 → 3 with weight 5
    ```

    is stored as:

    ```python
    graph[2] = [(5, 3)]
    ```

    Since nodes are labeled from `1` to `n`, we create arrays of size `n + 1` and leave index `0` unused.

    ## Distance Array

    ```python
    minDist = [float('inf')] * (n + 1)
    minDist[k] = 0
    ```

    `minDist[i]` represents the shortest known distance from the starting node `k` to node `i`.

    Initially:

    - `minDist[k] = 0`
    - All other distances are `inf`

    ## Min Heap

    Initialize the heap with:

    ```python
    minHeap = [(0, k)]
    ```

    Each element stores:

    ```text
    (distance, node)
    ```

    The min heap always pops the node with the **smallest current distance**.

    This replaces the `O(V)` linear search used in the naive implementation of Dijkstra.

    ## Process the Closest Node

    Each time, pop the closest candidate node:

    ```python
    curDist, curNode = heapq.heappop(minHeap)
    ```

    Before processing it, check whether this heap entry is outdated:

    ```python
    if curDist > minDist[curNode]:
        continue
    ```

    The same node may appear in the heap multiple times.

    For example:

    ```text
    First path to C:
    distance = 10
    → push (10, C)

    Later find a shorter path:
    distance = 5
    → push (5, C)

    Heap:
    (5, C)
    (10, C)
    ```

    After `(5, C)` is processed:

    ```text
    minDist[C] = 5
    ```

    When `(10, C)` is eventually popped:

    ```text
    curDist = 10
    minDist[C] = 5

    10 > 5
    ```

    So `(10, C)` is an outdated state and can be skipped.

    This is known as **lazy deletion**.

    ## Relaxation

    For every neighbor of the current node:

    ```python
    for nextDist, nextNode in graph[curNode]:
        newDist = curDist + nextDist
    ```

    Check whether going through `curNode` gives a shorter path:

    ```python
    if newDist < minDist[nextNode]:
        minDist[nextNode] = newDist
        heapq.heappush(minHeap, (newDist, nextNode))
    ```

    This is called **relaxation**.

    The core idea is:

    ```text
    new distance
    = shortest distance to current node
    + edge weight
    ```

    If:

    ```text
    newDist < current shortest distance
    ```

    then update the shortest distance and push the new state into the heap.

    ## Final Answer

    After Dijkstra finishes:

    ```python
    res = max(minDist[1:])
    ```

    `minDist[i]` contains the shortest time for the signal to reach node `i`.

    The entire network receives the signal only when the **farthest node** receives it, so we take the maximum shortest distance.

    We use:

    ```python
    minDist[1:]
    ```

    because index `0` is unused.

    If some node cannot be reached, its distance remains:

    ```python
    float('inf')
    ```

    Therefore:

    ```python
    return res if res != float('inf') else -1
    ```

    ## Complexity

    Let:

    - `V = n` be the number of nodes
    - `E = len(times)` be the number of edges

    ### Time Complexity

    ```text
    O((V + E) log V)
    ```

    Often written as:

    ```text
    O(E log V)
    ```

    Using a min heap avoids the `O(V)` search for the closest unvisited node in naive Dijkstra.

    ### Space Complexity

    ```text
    O(V + E)
    ```

    Mainly for the adjacency list, distance array, and heap.

    ## Dijkstra + Min Heap Template

    The general process is:

    ```text
    Initialize dist[source] = 0

    Push (0, source) into minHeap

    while minHeap:
        pop the node with the smallest distance

        if this heap entry is outdated:
            continue

        for each neighbor:
            calculate new distance

            if new distance is shorter:
                update dist
                push new state into heap
    ```

    ### Key Idea

    > The min heap always gives us the node with the smallest current candidate distance, while relaxation continuously improves the shortest distances.
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 28, 2026 |

---

[Back to Day 52 Graph X](index.md) · [All Algorithm notes](../index.md)
