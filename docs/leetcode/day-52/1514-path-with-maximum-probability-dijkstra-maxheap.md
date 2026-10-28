---
hide:
  - toc
---

# 1514. Path with Maximum Probability (Dijkstra + maxHeap)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/path-with-maximum-probability/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    This problem can be solved using a modified version of **Dijkstra's algorithm**.

    In standard Dijkstra, we try to find the **minimum distance** from the source node to every other node.

## Python solution

```python
import heapq

class Solution:
    def maxProbability(
        self,
        n: int,
        edges: List[List[int]],
        succProb: List[float],
        start_node: int,
        end_node: int
    ) -> float:

        maxProb = [0] * n
        maxProb[start_node] = 1

        graph = [[] for _ in range(n)]

        for (u, v), prob in zip(edges, succProb):
            graph[u].append((prob, v))
            graph[v].append((prob, u))

        maxHeap = [(-1, start_node)]

        while maxHeap:
            curProb, curNode = heapq.heappop(maxHeap)

            if -curProb < maxProb[curNode]:
                continue

            if curNode == end_node:
                return maxProb[end_node]

            for nextProb, nextNode in graph[curNode]:
                newProb = maxProb[curNode] * nextProb

                if newProb > maxProb[nextNode]:
                    maxProb[nextNode] = newProb
                    heapq.heappush(maxHeap, (-newProb, nextNode))

        return 0.0
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: Dijkstra + Max Heap

    This problem can be solved using a modified version of **Dijkstra's algorithm**.

    In standard Dijkstra, we try to find the **minimum distance** from the source node to every other node.

    In this problem, we instead want to find the **maximum probability** from `start_node` to every other node.

    ## Build the Graph

    The graph is **undirected**.

    `edges[i]` corresponds to `succProb[i]`, so we can use `zip()` to process them together.

    ```python
    graph = [[] for _ in range(n)]

    for (u, v), prob in zip(edges, succProb):
        graph[u].append((prob, v))
        graph[v].append((prob, u))
    ```

    Each entry stores:

    ```text
    (probability, nextNode)
    ```

    For example:

    ```text
    0 --0.5-- 1
    ```

    is stored as:

    ```python
    graph[0].append((0.5, 1))
    graph[1].append((0.5, 0))
    ```

    ## Define `maxProb`

    ```python
    maxProb = [0] * n
    maxProb[start_node] = 1
    ```

    `maxProb[i]` represents:

    > The maximum probability of reaching node `i` from `start_node`.
    >

    The starting node has probability `1` because reaching the starting node from itself requires no edge.

    All other nodes are initialized to `0` because we are trying to **maximize** the probability.

    ## Use a Max Heap

    Standard Dijkstra uses a **min heap** because it always processes the node with the smallest distance first.

    Here, we want to process the node with the **largest probability** first.

    Python's `heapq` is a min heap, so we store probabilities as negative values:

    ```python
    maxHeap = [(-1, start_node)]
    ```

    When popping:

    ```python
    curProb, curNode = heapq.heappop(maxHeap)
    ```

    the actual probability is:

    ```python
    -curProb
    ```

    For example:

    ```text
    Actual probability:    0.9    0.7    0.3

    Stored in heap:       -0.9   -0.7   -0.3
    ```

    Since:

    ```text
    -0.9 < -0.7 < -0.3
    ```

    the min heap pops `-0.9` first, which corresponds to the largest probability `0.9`.

    ## Skip Outdated Heap States

    A node may be pushed into the heap multiple times when a better path is discovered.

    Therefore:

    ```python
    if -curProb < maxProb[curNode]:
        continue
    ```

    If the probability stored in the popped heap state is smaller than the best probability already known for this node, this state is outdated and can be ignored.

    This is the same **lazy deletion** idea used in standard heap-based Dijkstra.

    ## Relaxation

    Suppose the current path is:

    ```text
    start → ... → curNode → nextNode
    ```

    The best probability of reaching `curNode` is:

    ```python
    maxProb[curNode]
    ```

    and the probability of the next edge is:

    ```python
    nextProb
    ```

    Therefore, the probability of the new path is:

    ```python
    newProb = maxProb[curNode] * nextProb
    ```

    If this gives a better probability:

    ```python
    if newProb > maxProb[nextNode]:
    ```

    update it:

    ```python
    maxProb[nextNode] = newProb
    ```

    and push the new state into the heap:

    ```python
    heapq.heappush(maxHeap, (-newProb, nextNode))
    ```

    ## Early Termination

    Because the heap always processes the node with the **largest current probability**, once `end_node` is popped:

    ```python
    if curNode == end_node:
        return maxProb[end_node]
    ```

    its maximum probability has already been determined.

    Therefore, we can return immediately.

    ## Why Dijkstra Works

    For every edge probability:

    ```text
    0 <= p <= 1
    ```

    extending a path can never increase its probability:

    ```text
    currentProb * p <= currentProb
    ```

    For example:

    ```text
    0.8 * 0.9 = 0.72
    ```

    Therefore, when the node with the largest probability is popped from the heap, no later path through a lower-probability state can produce a better probability for it.

    This gives us the greedy property required by Dijkstra.

    ## Comparison with Standard Dijkstra

    |  | Standard Dijkstra | LeetCode 1514 |
    | --- | --- | --- |
    | Goal | Minimum distance | Maximum probability |
    | Initial value | `inf` | `0` |
    | Start value | `0` | `1` |
    | Path calculation | `curDist + weight` | `curProb * edgeProb` |
    | Update condition | `<` | `>` |
    | Heap | Min Heap | Max Heap |
    | Python implementation | Positive distance | Negative probability |

    A useful way to remember the difference:

    ```python
    # Standard Dijkstra
    newDist = curDist + weight

    if newDist < minDist[nextNode]:
        minDist[nextNode] = newDist
    ```

    vs.

    ```python
    # LeetCode 1514
    newProb = curProb * edgeProb

    if newProb > maxProb[nextNode]:
        maxProb[nextNode] = newProb
    ```

    ## Complexity

    Let:

    - `V = n`
    - `E = len(edges)`

    Building the graph takes:

    ```text
    O(E)
    ```

    Each heap operation takes:

    ```text
    O(log V)
    ```

    Therefore:

    ```text
    Time:  O((V + E) log V)
    Space: O(V + E)
    ```

    For a connected graph, the time complexity is commonly simplified to:

    ```text
    O(E log V)
    ```

    ## Key Takeaway

    Dijkstra is not limited to finding the shortest sum of edge weights.

    The key idea is to repeatedly process the currently **best** state:

    ```text
    743  → smallest total distance
    1631 → smallest maximum edge difference
    1514 → largest path probability
    ```

    For LeetCode 1514:

    ```text
    Path probability = product of edge probabilities
    ```

    so the relaxation rule is:

    ```python
    newProb = curProb * edgeProb
    ```

    Since we want the **maximum** probability, we need a **max heap**.

    Python's `heapq` is a min heap, so we simulate a max heap by storing:

    ```python
    -probability
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 28, 2026 |

---

[Back to Day 52 Graph X](index.md) · [All Algorithm notes](../index.md)
