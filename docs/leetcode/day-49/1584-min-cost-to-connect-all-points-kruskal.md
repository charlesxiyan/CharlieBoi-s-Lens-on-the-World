---
hide:
  - toc
---

# 1584. Min Cost to Connect All Points (Kruskal)

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/min-cost-to-connect-all-points/description/)

!!! tip "Core idea"

    This problem asks us to connect all points while minimizing the total connection cost.

## Python solution

```python
from typing import List

class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))

    def find(self, node):
        if node != self.parent[node]:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]

    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        # Different components:
        # this edge can be safely added to the MST
        if root1 != root2:
            self.parent[root1] = root2
            return True

        # Same component:
        # adding this edge would create a cycle
        return False

class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        n = len(points)
        edges = []

        # Build all possible edges
        for i in range(n):
            for j in range(i + 1, n):
                x1, y1 = points[i]
                x2, y2 = points[j]

                dist = abs(x1 - x2) + abs(y1 - y2)

                edges.append((dist, i, j))

        # Kruskal: process edges from smallest to largest
        edges.sort()

        uf = UnionFind(n)

        res = 0
        edgeCounter = 0

        for dist, u, v in edges:
            # Only select the edge if it connects
            # two different components
            if uf.union(u, v):
                res += dist
                edgeCounter += 1

                # An MST with n nodes contains exactly n - 1 edges
                if edgeCounter == n - 1:
                    break

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem asks us to connect all points while minimizing the total connection cost.

    The cost between two points is their **Manhattan distance**:

    ```text
    distance = |x1 - x2| + |y1 - y2|
    ```

    We need to satisfy two requirements:

    - All points must be connected.
    - The total cost must be as small as possible.

    This is a classic **Minimum Spanning Tree (MST)** problem.

    We can solve it using **Kruskal's Algorithm + Union Find**.

    ## Kruskal's Algorithm

    The core idea of Kruskal's algorithm is:

    ```text
    Build all possible edges

            ↓

    Sort all edges by weight
    from smallest to largest

            ↓

    Consider edges one by one

            ↓

    Does this edge connect
    two different components?

          /           \
        Yes            No
         ↓              ↓
    Take the edge     Skip it
         ↓
    Union the two components

            ↓

    Stop after selecting n - 1 edges
    ```

    In this problem, the **weight** of an edge is the Manhattan distance between two points.

    Suppose we have `n` points.

    Every pair of points can be connected, so we consider every pair:

    ```python
    for i in range(n):
        for j in range(i + 1, n):
    ```

    We start `j` from:

    ```python
    i + 1
    ```

    because the distance between `i` and `j` is the same as the distance between `j` and `i`.

    For example:

    ```text
    distance(1, 3) = distance(3, 1)
    ```

    Therefore, we only need to generate each edge once.

    For every pair of points, calculate:

    ```text
    dist = |x1 - x2| + |y1 - y2|
    ```

    and store:

    ```python
    (dist, i, j)
    ```

    For example:

    ```text
    (4, 0, 1)
    ```

    means:

    ```text
    point 0 ←── cost 4 ──→ point 1
    ```

    So every element in `edges` has the format:

    ```text
    (weight, node1, node2)
    ```

    After generating all edges, we use:

    ```python
    edges.sort()
    ```

    Since each edge is stored as:

    ```python
    (dist, i, j)
    ```

    Python sorts tuples from left to right.

    Therefore, it first compares `dist`.

    For example:

    ```text
    Before sorting:

    (7, 0, 3)
    (4, 0, 1)
    (13, 0, 2)
    (3, 1, 3)

            ↓

    edges.sort()

            ↓

    (3, 1, 3)
    (4, 0, 1)
    (7, 0, 3)
    (13, 0, 2)
    ```

    So the edges are processed from the **smallest distance to the largest distance**.

    ### Why is this necessary?

    The goal of Kruskal's algorithm is to build the MST using the cheapest possible edges.

    Therefore, we always want to ask:

    ```text
    Can I take the cheapest remaining edge
    without creating a cycle?
    ```

    If yes, take it.

    If no, skip it and consider the next cheapest edge.

    For example, suppose the available edges are:

    ```text
    A --2-- B
    A --5-- C
    B --3-- C
    ```

    We process:

    ```text
    2 → A-B
    3 → B-C
    5 → A-C
    ```

    First:

    ```text
    A --2-- B
    ```

    Take it.

    Then:

    ```text
    B --3-- C
    ```

    Take it.

    Now all three nodes are connected:

    ```text
    A --2-- B --3-- C
    ```

    Total:

    ```text
    2 + 3 = 5
    ```

    We do not need:

    ```text
    A --5-- C
    ```

    because `A` and `C` are already connected.

    Adding it would create a cycle and increase the total cost unnecessarily.

    This is why Kruskal processes edges:

    ```text
    smallest weight
          ↓
          ↓
          ↓
    largest weight
    ```

    A useful way to remember Kruskal is:

    > **Always try the cheapest edge first. Keep it only if it connects two previously disconnected components.**
    >

    Sorting the edges is not enough.

    When considering an edge:

    ```text
    u —— v
    ```

    we need to know whether `u` and `v` are already connected.

    If they are already connected, adding this edge would create a cycle.

    This is exactly what **Union Find** is good at.

    ## `find`

    The `find` function returns the root of a node:

    ```python
    def find(self, node):
        if node != self.parent[node]:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    ```

    We also use **path compression**:

    ```python
    self.parent[node] = self.find(self.parent[node])
    ```

    so future `find` operations become faster.

    Our `union()` function is:

    ```python
    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            self.parent[root1] = root2
            return True

        return False
    ```

    The Boolean return value is very useful for Kruskal.

    There are two possible situations.

    ### Situation 1: `root1 != root2`

    Suppose:

    ```text
    Component 1: A -- B

    Component 2: C -- D
    ```

    Now we consider:

    ```text
    B -- C
    ```

    We get:

    ```text
    find(B) != find(C)
    ```

    which means `B` and `C` belong to different connected components.

    Therefore, this edge can safely connect the two components:

    ```text
    A -- B -- C -- D
    ```

    No cycle is created.

    So:

    ```python
    self.parent[root1] = root2
    return True
    ```

    Here `True` means:

    > This edge successfully connected two previously disconnected components, so Kruskal should select this edge.
    >

    Therefore:

    ```python
    if uf.union(u, v):
        res += dist
    ```

    ### Situation 2: `root1 == root2`

    Suppose we already have:

    ```text
    A -- B -- C
    ```

    Now we consider another edge:

    ```text
    A ----- C
    ```

    Since `A` and `C` are already connected:

    ```text
    find(A) == find(C)
    ```

    Adding:

    ```text
    A ----- C
    ```

    would create:

    ```text
    A ----- C
     \     /
      \   /
        B
    ```

    which contains a cycle.

    Therefore:

    ```python
    return False
    ```

    Here `False` means:

    > These two nodes are already connected. Do not select this edge.
    >

    So this code:

    ```python
    if uf.union(u, v):
        res += dist
        edgeCounter += 1
    ```

    can be understood as:

    ```text
    Try to union u and v

            ↓

    union returns True?
          /       \
        Yes        No
         ↓          ↓
    Take edge    Skip edge
         ↓
    res += dist
    ```

    This combines **cycle detection and union** into one function.

    When:

    ```python
    union(u, v)
    ```

    returns `False`, it means:

    ```text
    u and v are already in the same connected component.
    ```

    Therefore:

    > **Adding the current edge would create a cycle.**
    >

    It does not mean that the Union Find tree itself is "a cycle."

    Union Find internally always maintains a forest-like parent structure.

    The cycle refers to the **original graph if we were to add this edge**.

    This is one of the most important properties of a tree.

    A tree with:

    ```text
    n nodes
    ```

    always has exactly:

    ```text
    n - 1 edges
    ```

    For example:

    ```text
    1 node  → 0 edges

    2 nodes → 1 edge

    3 nodes → 2 edges

    4 nodes → 3 edges

    5 nodes → 4 edges
    ```

    Why?

    Suppose we start with `n` completely separate nodes:

    ```text
    A    B    C    D
    ```

    There are initially:

    ```text
    4 connected components
    ```

    Every valid edge selected by Kruskal merges **two different components**:

    ```text
    A -- B    C    D
    ```

    Now there are:

    ```text
    3 components
    ```

    Add another edge:

    ```text
    A -- B -- C    D
    ```

    Now:

    ```text
    2 components
    ```

    Add another:

    ```text
    A -- B -- C -- D
    ```

    Now:

    ```text
    1 component
    ```

    We started with:

    ```text
    n components
    ```

    and every selected edge reduces the number of components by exactly `1`.

    To go from:

    ```text
    n components
    ```

    to:

    ```text
    1 component
    ```

    we need:

    ```text
    n - 1
    ```

    successful unions.

    Therefore, an MST always contains exactly:

    ```text
    n - 1 edges
    ```

    We use:

    ```python
    edgeCounter = 0
    ```

    Every time `union()` returns `True`, we successfully add one edge to the MST:

    ```python
    if uf.union(u, v):
        res += dist
        edgeCounter += 1
    ```

    Once:

    ```python
    edgeCounter == n - 1
    ```

    we know:

    ```text
    All n nodes are connected
    +
    No cycle exists
    ```

    Therefore, the MST is complete.

    We can immediately:

    ```python
    break
    ```

    There is no reason to continue checking the remaining edges.

    So:

    ```python
    if edgeCounter == n - 1:
        break
    ```

    is an **early stopping condition**.

    Because we only increment `edgeCounter` when:

    ```python
    uf.union(u, v)
    ```

    returns `True`.

    That means every selected edge connects two previously disconnected components.

    Therefore, none of the selected edges creates a cycle.

    So after selecting `n - 1` valid edges, we have:

    ```text
    n nodes
    +
    n - 1 edges
    +
    all nodes connected
    +
    no cycle
    ```

    which is exactly a tree.

    Since we always considered edges from the smallest weight to the largest weight, this tree is a **Minimum Spanning Tree**.

    The entire algorithm can be summarized as:

    ```text
    All points
        ↓
    Generate every possible edge
        ↓
    Calculate Manhattan distance
        ↓
    Store (distance, u, v)
        ↓
    Sort edges by distance
        ↓
    Smallest edge first
        ↓
    For each edge (u, v):
        ↓
    union(u, v)
       /      \
     True     False
      ↓         ↓
    Take      Skip
    edge      edge
      ↓
    res += distance
      ↓
    edgeCounter += 1
      ↓
    edgeCounter == n - 1?
       /       \
     Yes        No
      ↓          ↓
    Done       Continue
    ```

    A useful way to distinguish the two MST algorithms is:

    ```text
    Prim
    → Focuses on nodes
    → Grow one tree
    → Every round adds one new node

    Kruskal
    → Focuses on edges
    → Start with many separate components
    → Every successful union merges two components
    ```

    For this problem:

    ```text
    Prim:
    Choose the cheapest node to connect to the current MST.

    Kruskal:
    Choose the cheapest edge that does not create a cycle.
    ```

    There are `n` points.

    Since every pair of points can form an edge, the number of edges is:

    ```text
    E = n(n - 1) / 2
    ```

    which is:

    ```text
    O(n²)
    ```

    Building all edges:

    ```text
    O(n²)
    ```

    Sorting all edges:

    ```text
    O(E log E)
    ```

    Since:

    ```text
    E = O(n²)
    ```

    the sorting cost is approximately:

    ```text
    O(n² log n)
    ```

    Union Find operations are very close to constant time with path compression.

    Therefore:

    ```text
    Time Complexity: O(n² log n)

    Space Complexity: O(n²)
    ```

    The `O(n²)` space comes mainly from storing all possible edges.



    The three most important ideas in this solution are:

    **Why sort the edges?**

    ```text
    Kruskal is greedy.

    Always consider the cheapest remaining edge first.
    ```

    **Why does `union()` return `True` or `False`?**

    ```text
    True
    → u and v were in different components
    → this edge does not create a cycle
    → select it

    False
    → u and v were already connected
    → this edge would create a cycle
    → skip it
    ```

    **Why stop at `n - 1` edges?**

    ```text
    A tree with n nodes has exactly n - 1 edges.

    Every successful union reduces
    the number of components by 1:

    n components
        ↓
    n - 1
        ↓
    n - 2
        ↓
    ...
        ↓
    1 component

    Therefore:

    n - 1 successful unions
    = all nodes connected
    = MST complete
    ```

    The shortest way to remember **Kruskal** is:

    > **Sort edges from cheapest to most expensive, take an edge if it connects two different components, and stop after selecting `n - 1` edges.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 25, 2026 |

---

[Back to Day 49 Graph VII](index.md) · [All Algorithm notes](../index.md)
