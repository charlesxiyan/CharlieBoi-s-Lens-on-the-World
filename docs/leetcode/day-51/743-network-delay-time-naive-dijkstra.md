---
hide:
  - toc
---

# 743. Network Delay Time (Naive Dijkstra)

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/network-delay-time/description/)

!!! tip "Core idea"

    Use **Dijkstra's Algorithm** to find the shortest distance from the starting node `k` to every other node.

## Python solution

```python
# Deque
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        graph = defaultdict(list)
        for u,v,w in times:
            graph[u].append((v,w))

        distance = [inf] * (n+1)
        distance[k] = 0
        # visited = [k]
        in_queue = [False] * (n+1)
        in_queue[k] = True


        queue = deque()
        queue.append(k)
        while queue:
            cur = queue.popleft()
            in_queue[cur] = False
            for nxt,w in graph[cur]:
                if distance[cur] + w < distance[nxt]:
                    distance[nxt] = distance[cur] + w
                    if not in_queue[nxt]:
                        queue.append(nxt)
                        in_queue[nxt] = True
        ans = max(distance[1:])
        return ans if ans < inf else -1
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Dijkstra's Algorithm** to find the shortest distance from the starting node `k` to every other node.

    This version uses:

    - **Adjacency Matrix**
    - **Linear Scan**
    - **Visited Array**

    It is the **naive Dijkstra** implementation without a min-heap.

    The problem asks how long it takes for the signal to reach all nodes.

    So after finding the shortest distance from `k` to every node, the answer is:

    ```python
    max(minDist[1:])
    ```

    because the total delay is determined by the node that receives the signal last.

    If any node is unreachable, return `-1`.

    ## Graph Representation

    Each edge:

    ```text
    [u, v, w]
    ```

    means:

    ```text
    u --w--> v
    ```

    We use an adjacency matrix:

    ```python
    graph = [[float('inf')] * (n + 1) for _ in range(n + 1)]
    ```

    Then:

    ```python
    for u, v, w in times:
        graph[u][v] = w
    ```

    So:

    ```python
    graph[u][v]
    ```

    represents the travel time from node `u` to node `v`.

    If:

    ```python
    graph[u][v] == float('inf')
    ```

    then there is no direct edge from `u` to `v`.

    ## `minDist`

    Define:

    ```python
    minDist[i]
    ```

    as:

    > The shortest distance currently known from the starting node `k` to node `i`.
    >

    Initially:

    ```python
    minDist = [float('inf')] * (n + 1)
    minDist[k] = 0
    ```

    The starting node has distance `0` from itself.

    All other nodes are initially unreachable, so their distances are set to infinity.

    Example:

    ```text
    k = 2

    node:       1    2    3    4
    minDist:   inf   0   inf  inf
    ```

    ## `visit`

    Define:

    ```python
    visit[i]
    ```

    as:

    > Whether the shortest distance to node `i` has already been finalized.
    >

    Initially:

    ```python
    visit = [False] * (n + 1)
    ```

    When:

    ```python
    visit[i] = True
    ```

    it means the shortest distance from `k` to node `i` has been determined and will no longer change.

    ## Step 1: Find the Closest Unvisited Node

    At each round, find the unvisited node with the smallest current distance:

    ```python
    minVal = float('inf')
    cur = -1

    for node in range(1, n + 1):
        if not visit[node] and minDist[node] < minVal:
            minVal = minDist[node]
            cur = node
    ```

    This is the key step of naive Dijkstra.

    Among all unvisited nodes, choose the one with the smallest:

    ```python
    minDist[node]
    ```

    Then:

    ```python
    cur
    ```

    is the next node whose shortest distance can be finalized.

    ## Why Can We Finalize `cur`?

    Dijkstra assumes all edge weights are non-negative.

    If `cur` currently has the smallest distance among all unvisited nodes, no other unvisited path can later produce a shorter route to `cur`.

    Therefore:

    ```python
    visit[cur] = True
    ```

    and:

    ```python
    minDist[cur]
    ```

    is now finalized.

    ## Why `cur == -1`?

    We initialize:

    ```python
    cur = -1
    ```

    and only update it when:

    ```python
    minDist[node] < minVal
    ```

    Since:

    ```python
    minVal = float('inf')
    ```

    a node whose distance is also `inf` will not be selected because:

    ```text
    inf < inf
    ```

    is `False`.

    Therefore, if:

    ```python
    cur == -1
    ```

    after scanning all nodes, it means every remaining unvisited node has:

    ```python
    minDist[node] == float('inf')
    ```

    In other words:

    > No remaining node is reachable from the starting node.
    >

    So we can stop early:

    ```python
    if cur == -1:
        break
    ```

    ## Step 2: Relax the Neighbors

    After finalizing `cur`, try to update all nodes directly reachable from `cur`.

    Suppose:

    ```text
    k → ... → cur → node
    ```

    We already know:

    ```python
    minDist[cur]
    ```

    and the edge:

    ```python
    graph[cur][node]
    ```

    Then the new possible distance to `node` is:

    ```python
    minDist[cur] + graph[cur][node]
    ```

    If this is smaller than the current:

    ```python
    minDist[node]
    ```

    we update it.

    In the code:

    ```python
    if (
        not visit[node]
        and graph[cur][node] != float('inf')
        and graph[cur][node] + minDist[cur] < minDist[node]
    ):
        minDist[node] = graph[cur][node] + minDist[cur]
    ```

    This process is called **relaxation**.

    ## Relaxation Formula

    The core transition is:

    ```python
    minDist[node] = min(
        minDist[node],
        minDist[cur] + graph[cur][node]
    )
    ```

    It compares two possibilities:

    1. Keep the previously known shortest distance.
    2. Reach `node` through `cur`.

    If going through `cur` is shorter, update the distance.

    ## Hand Simulation

    Suppose:

    ```python
    times = [
        [2, 1, 1],
        [2, 3, 1],
        [3, 4, 1]
    ]

    n = 4
    k = 2
    ```

    The graph is:

    ```text
        1
        ↑
        1
        |
        2 --1--> 3 --1--> 4
    ```

    Initially:

    ```text
    node:       1     2     3     4
    minDist:   inf    0    inf   inf
    visit:      F     F     F     F
    ```

    ### Round 1

    The closest unvisited node is:

    ```text
    cur = 2
    ```

    because:

    ```text
    minDist[2] = 0
    ```

    Mark it visited:

    ```text
    visit[2] = True
    ```

    Relax its neighbors.

    For:

    ```text
    2 → 1
    ```

    we get:

    ```text
    0 + 1 = 1
    ```

    so:

    ```text
    minDist[1] = 1
    ```

    For:

    ```text
    2 → 3
    ```

    we get:

    ```text
    0 + 1 = 1
    ```

    so:

    ```text
    minDist[3] = 1
    ```

    Now:

    ```text
    node:       1    2    3    4
    minDist:    1    0    1   inf
    visit:      F    T    F    F
    ```

    ### Round 2

    Among unvisited nodes:

    ```text
    1 → distance 1
    3 → distance 1
    4 → distance inf
    ```

    The scan finds node `1` first:

    ```text
    cur = 1
    ```

    Mark:

    ```text
    visit[1] = True
    ```

    Node `1` has no outgoing edges, so nothing changes.

    ### Round 3

    The closest remaining node is:

    ```text
    cur = 3
    ```

    Mark:

    ```text
    visit[3] = True
    ```

    There is an edge:

    ```text
    3 --1--> 4
    ```

    So:

    ```text
    minDist[4]
    = min(inf, minDist[3] + 1)
    = min(inf, 1 + 1)
    = 2
    ```

    Now:

    ```text
    node:       1    2    3    4
    minDist:    1    0    1    2
    visit:      T    T    T    F
    ```

    ### Round 4

    The last node is:

    ```text
    cur = 4
    ```

    After processing it:

    ```text
    minDist = [1, 0, 1, 2]
    ```

    excluding index `0`.

    ## Final Answer

    The signal reaches each node after:

    ```text
    node 1: 1
    node 2: 0
    node 3: 1
    node 4: 2
    ```

    The last node receives the signal after:

    ```python
    max(minDist[1:])
    ```

    which is:

    ```text
    2
    ```

    So the answer is:

    ```python
    2
    ```

    ## Why Take the Maximum?

    Dijkstra gives us the shortest distance from `k` to every node.

    But the problem asks:

    > How long until every node receives the signal?
    >

    All nodes have received the signal only when the slowest node receives it.

    Therefore:

    ```python
    res = max(minDist[1:])
    ```

    ## Unreachable Nodes

    If one node cannot be reached from `k`, its distance remains:

    ```python
    float('inf')
    ```

    Therefore:

    ```python
    res = max(minDist[1:])
    ```

    will also be:

    ```python
    float('inf')
    ```

    So:

    ```python
    return -1 if res == float('inf') else res
    ```

    ## Key Idea of Naive Dijkstra

    Each round performs two operations:

    ### Find

    Find the closest unvisited node:

    ```text
    Unvisited nodes
            ↓
    smallest minDist
            ↓
           cur
    ```

    ### Relax

    Use `cur` to update its neighbors:

    ```text
    minDist[node]
        =
    min(
        old distance,
        minDist[cur] + edge weight
    )
    ```

    Then repeat.

    Memory:

    > **Find the closest unvisited node → finalize it → relax its neighbors → repeat.**
    >

    ## Complexity

    The outer loop runs at most `n` times.

    Each time, we scan all `n` nodes to find the closest unvisited node:

    ```text
    O(n)
    ```

    Then we scan all `n` nodes again to relax neighbors:

    ```text
    O(n)
    ```

    Therefore:

    ```text
    Time: O(n²)
    ```

    The adjacency matrix requires:

    ```text
    Space: O(n²)
    ```

    So overall:

    ```text
    Time: O(n²)
    Space: O(n²)
    ```

    ## Code

    ```python
    class Solution:
        def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
            minDist = [float('inf')] * (n + 1)
            visit = [False] * (n + 1)
            minDist[k] = 0

            graph = [[float('inf')] * (n + 1) for _ in range(n + 1)]

            for u, v, w in times:
                graph[u][v] = w

            for _ in range(n):
                minVal = float('inf')
                cur = -1

                for node in range(1, n + 1):
                    if not visit[node] and minDist[node] < minVal:
                        minVal = minDist[node]
                        cur = node

                if cur == -1:
                    break

                visit[cur] = True

                for node in range(1, n + 1):
                    if (
                        not visit[node]
                        and graph[cur][node] != float('inf')
                        and graph[cur][node] + minDist[cur] < minDist[node]
                    ):
                        minDist[node] = graph[cur][node] + minDist[cur]

            res = max(minDist[1:])

            return -1 if res == float('inf') else res
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 28, 2026 |

---

[Back to Day 51 Graph IX](index.md) · [All Algorithm notes](../index.md)
