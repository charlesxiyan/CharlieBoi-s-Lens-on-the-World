---
hide:
  - toc
---

# 1584. Min Cost to Connect All Points (Prim)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/min-cost-to-connect-all-points/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    This problem asks us to connect all points with the minimum possible total cost.

## Python solution

```python
from typing import List

class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        n = len(points)

        visit = [False] * n
        minDist = [float('inf')] * n
        minDist[0] = 0

        res = 0

        for _ in range(n):
            # No node has been selected in this round
            u = -1

            # Find the unvisited node with the smallest minDist
            for i in range(n):
                if not visit[i] and (u == -1 or minDist[i] < minDist[u]):
                    u = i

            # Add u to the MST
            visit[u] = True
            res += minDist[u]

            # Update minDist for all unvisited nodes
            x, y = points[u]

            for v in range(n):
                if not visit[v]:
                    a, b = points[v]

                    minDist[v] = min(
                        minDist[v],
                        abs(x - a) + abs(y - b)
                    )

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem asks us to connect all points with the minimum possible total cost.

    The cost of connecting two points is their **Manhattan distance**:

    ```text
    distance = |x1 - x2| + |y1 - y2|
    ```

    We need to:

    - Connect all points.
    - Avoid unnecessary edges.
    - Minimize the total edge cost.

    This is a classic **Minimum Spanning Tree (MST)** problem.

    We can solve it using **Prim's Algorithm**.

    ## Prim's Algorithm

    Prim's algorithm builds one Minimum Spanning Tree by gradually adding nodes.

    The basic idea is:

    ```text
    Start with one node
            ↓
    Find the cheapest unvisited node
    that can connect to the current MST
            ↓
    Add it to the MST
            ↓
    Update the connection costs
    of the remaining nodes
            ↓
    Repeat until all nodes are included
    ```

    Unlike Kruskal's algorithm, Prim focuses on **adding nodes** rather than sorting and selecting edges.

    ## Key Data Structures

    We need two arrays:

    ```python
    visit = [False] * n
    minDist = [float('inf')] * n
    ```

    ### `visit[i]`

    ```text
    visit[i] = True
    ```

    means point `i` has already been added to the MST.

    ```text
    visit[i] = False
    ```

    means point `i` has not been added yet.

    ### `minDist[i]`

    This is the most important part of the algorithm.

    ```text
    minDist[i]
    ```

    represents:

    > The minimum cost required to connect point `i` to any point already inside the current MST.
    >

    For example, suppose the current MST contains:

    ```text
    {0, 2, 4}
    ```

    Then:

    ```text
    minDist[3]
    ```

    is:

    ```text
    min(
        distance(0, 3),
        distance(2, 3),
        distance(4, 3)
    )
    ```

    So `minDist[i]` is **not** simply the distance from `i` to the most recently added point.

    It is the minimum distance from `i` to the **entire current MST**.

    ## Initialization

    We initialize:

    ```python
    visit = [False] * n
    minDist = [float('inf')] * n
    ```

    Then:

    ```python
    minDist[0] = 0
    ```

    Why?

    Prim's algorithm can start from any node.

    We choose point `0` as the starting point.

    Since the first point does not need an edge to connect to the MST:

    ```text
    cost = 0
    ```

    Therefore:

    ```python
    minDist[0] = 0
    ```

    This guarantees that point `0` will be selected first.

    ## Step 1: Find the Next Node to Add

    For every round:

    ```python
    u = -1
    ```

    Here, `u` represents:

    > The next node that will be added to the MST.
    >
    - `1` is simply a sentinel value meaning:

    ```text
    No candidate has been selected yet.
    ```

    Then we search through all nodes:

    ```python
    for i in range(n):
        if not visit[i] and (u == -1 or minDist[i] < minDist[u]):
            u = i
    ```

    We only consider:

    ```python
    not visit[i]
    ```

    because nodes already inside the MST should not be selected again.

    Among all unvisited nodes, we choose the one with the smallest:

    ```text
    minDist[i]
    ```

    This means we always choose the cheapest possible way to expand the current MST.

    ## Why Do We Need `u = -1`?

    At the beginning of each round, we have not selected any candidate yet.

    Suppose:

    ```text
    visit   = [True, False, False, False]

    minDist = [0,    5,     2,     8]
    ```

    Initially:

    ```text
    u = -1
    ```

    When we encounter point `1`:

    ```text
    u == -1
    ```

    so we first set:

    ```text
    u = 1
    ```

    Then point `2` is considered:

    ```text
    minDist[2] = 2
    minDist[1] = 5
    ```

    Since:

    ```text
    2 < 5
    ```

    we update:

    ```text
    u = 2
    ```

    Point `3` has:

    ```text
    minDist[3] = 8
    ```

    so `u` remains `2`.

    Therefore, point `2` is selected.

    ## Step 2: Add `u` to the MST

    Once we find the cheapest node:

    ```python
    visit[u] = True
    res += minDist[u]
    ```

    `visit[u] = True` means:

    ```text
    u is now part of the MST.
    ```

    And:

    ```python
    res += minDist[u]
    ```

    adds the cost of connecting `u` to the current MST.

    ## Step 3: Update `minDist`

    After adding `u`, this new node may provide a cheaper way to connect some remaining nodes.

    Suppose:

    ```text
    u = newly added node
    v = an unvisited node
    ```

    We calculate their Manhattan distance:

    ```python
    dist = abs(x1 - x2) + abs(y1 - y2)
    ```

    Then:

    ```python
    minDist[v] = min(minDist[v], dist)
    ```

    This is very important.

    We are comparing:

    ```text
    minDist[v]
            ↓
    The cheapest connection from v
    to the OLD MST

    vs.

    distance(u, v)
            ↓
    The connection from v
    to the NEWLY ADDED node u
    ```

    So:

    ```text
    new minDist[v]

    = min(
        old minDist[v],
        distance(u, v)
    )
    ```

    Because `u` has just joined the MST, `distance(u, v)` is now another possible way for `v` to connect to the MST.

    ## Example

    Consider:

    ```python
    points = [[0,0], [2,2], [3,10], [5,2], [7,0]]
    ```

    Initially:

    ```text
    MST = {}

    visit:
    [F, F, F, F, F]

    minDist:
    [0, ∞, ∞, ∞, ∞]
    ```

    ### Round 1

    The smallest `minDist` belongs to point `0`.

    Add point `0`:

    ```text
    MST = {0}

    res = 0
    ```

    Now calculate distances from point `0`:

    ```text
    0 → 1 = 4
    0 → 2 = 13
    0 → 3 = 7
    0 → 4 = 7
    ```

    Therefore:

    ```text
    minDist:

    [0, 4, 13, 7, 7]
    ```

    ### Round 2

    Among unvisited nodes:

    ```text
    point 1 → 4
    point 2 → 13
    point 3 → 7
    point 4 → 7
    ```

    The minimum is point `1`.

    Add it:

    ```text
    MST = {0, 1}

    res = 0 + 4 = 4
    ```

    Now use point `1` to update the remaining nodes.

    For example:

    ```text
    distance(1, 3)
    = |2 - 5| + |2 - 2|
    = 3
    ```

    Previously:

    ```text
    minDist[3] = 7
    ```

    Now:

    ```text
    minDist[3] = min(7, 3)
               = 3
    ```

    This demonstrates the meaning of `minDist`:

    ```text
    Before:

    point 3 ← 7 → current MST

    After adding point 1:

    point 3 ← 3 → point 1

    Since point 1 is already in the MST:

    minDist[3] becomes 3.
    ```

    ## Why Is Prim Greedy?

    At every step, we choose:

    > The cheapest possible edge that connects the current MST to a new node.
    >

    We never choose a more expensive connection if a cheaper valid connection is available.

    Therefore, the MST grows like:

    ```text
    Current MST
         |
         | cheapest connection
         ↓
     New Node
    ```

    until all nodes are included.

    ## Solution



    ## Complexity Analysis

    Let `n` be the number of points.

    For each of the `n` rounds:

    - We scan all `n` nodes to find the minimum `minDist`.
    - We scan all `n` nodes again to update their distances.

    Therefore:

    ```text
    Time Complexity: O(n²)
    ```

    We use:

    ```text
    visit   → O(n)
    minDist → O(n)
    ```

    Therefore:

    ```text
    Space Complexity: O(n)
    ```

    An important advantage of this implementation is that we **do not explicitly construct all O(n²) edges**.

    ## Key Takeaway

    The most important thing to understand is the meaning of:

    ```python
    minDist[i]
    ```

    It means:

    > **The minimum cost to connect point `i` to the current MST.**
    >

    Therefore, Prim repeatedly performs three steps:

    ```text
    1. Select

       Find the unvisited node u
       with the smallest minDist[u]

                    ↓

    2. Add

       visit[u] = True
       res += minDist[u]

                    ↓

    3. Update

       For every unvisited v:

       minDist[v]
           =
       min(
           minDist[v],
           distance(u, v)
       )
    ```

    A simple way to remember Prim is:

    > **Choose the cheapest node to connect → add it to the MST → use the new node to update the remaining connection costs.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 25, 2026 |

---

[Back to Day 49 Graph VII](index.md) · [All Algorithm notes](../index.md)
