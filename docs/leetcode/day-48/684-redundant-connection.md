---
hide:
  - toc
---

# 684. Redundant Connection

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/redundant-connection/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        This problem is a classic **Union Find (Disjoint Set Union)** problem.

        We are given an undirected graph that originally forms a tree, but one extra edge has been added.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem is a classic **Union Find (Disjoint Set Union)** problem.

    We are given an undirected graph that originally forms a tree, but one extra edge has been added.

    Our goal is to find the edge that creates a **cycle**.

    The key observation is:

    > Before adding an edge `[u, v]`, if `u` and `v` are already in the same set, then there is already a path connecting them. Adding `[u, v]` will create a cycle.
    >

    Therefore, for every edge:

    ```text
    1. Check whether u and v are already connected.
    2. If yes → this edge is redundant.
    3. If no  → union their two sets.
    ```

    ## Why Union Find?

    Union Find is designed to efficiently answer:

    ```text
    Are two nodes already connected?
    ```

    We maintain a `parent` array where nodes in the same connected component eventually have the same root.

    Therefore:

    ```python
    find(u) == find(v)
    ```

    means:

    ```text
    u and v are already connected.
    ```

    ## Step 1: Initialize Union Find

    The nodes in this problem are numbered:

    ```text
    1, 2, ..., n
    ```

    Therefore, we use:

    ```python
    self.parent = list(range(n + 1))
    ```

    For example:

    ```text
    n = 5

    parent = [0, 1, 2, 3, 4, 5]
    ```

    Index `0` is unused.

    Initially, every node belongs to its own set:

    ```text
    {1} {2} {3} {4} {5}
    ```

    ## Step 2: Find

    `find(node)` returns the root of the set containing `node`.

    ```python
    def find(self, node):
        if self.parent[node] != node:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    ```

    We also use **path compression**:

    ```python
    self.parent[node] = self.find(self.parent[node])
    ```

    For example, if we originally have:

    ```text
    1 → 2 → 3 → 4
    ```

    after calling `find(1)`, the structure can become:

    ```text
    1 ──→ 4
    2 ──→ 4
    3 ──→ 4
    ```

    This makes future `find` operations faster.

    ## Step 3: Union

    To merge two nodes:

    ```python
    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            self.parent[root1] = root2
    ```

    We first find their roots.

    If:

    ```text
    root1 != root2
    ```

    the two nodes belong to different connected components, so we merge them.

    For example:

    ```text
    Set 1: {1, 2}
    Set 2: {3, 4}
    ```

    After:

    ```python
    union(2, 3)
    ```

    they become:

    ```text
    {1, 2, 3, 4}
    ```

    ## Step 4: Check Whether Two Nodes Are Already Connected

    We can define:

    ```python
    def isSame(self, node1, node2):
        return self.find(node1) == self.find(node2)
    ```

    If they have the same root, they are already connected.

    This is the most important operation for this problem.

    ## Why Does `isSame(u, v)` Detect a Cycle?

    Suppose the graph currently looks like:

    ```text
    1 ----- 2
     \
      \
       3
    ```

    There is already a path between `2` and `3`:

    ```text
    2 → 1 → 3
    ```

    Therefore:

    ```python
    find(2) == find(3)
    ```

    Now suppose the next edge is:

    ```text
    [2, 3]
    ```

    If we add it:

    ```text
    1 ----- 2
     \     /
      \   /
        3
    ```

    we create a cycle.

    Therefore:

    ```text
    If u and v are already connected
    +
    we add another edge between u and v
    =
    a cycle is created
    ```

    So the redundant edge can be detected **before calling `union()`**.

    ## Algorithm

    For every edge `[u, v]`:

    ```python
    if uf.isSame(u, v):
        return [u, v]

    uf.union(u, v)
    ```

    The order is important.

    We must first check:

    ```python
    uf.isSame(u, v)
    ```

    and then perform:

    ```python
    uf.union(u, v)
    ```

    If we called `union()` first, `u` and `v` would obviously become connected, so we would lose the information needed to determine whether they were already connected before adding the edge.

    ## Hand Simulation

    Consider:

    ```python
    edges = [
        [1, 2],
        [1, 3],
        [2, 3]
    ]
    ```

    Initially:

    ```text
    {1} {2} {3}
    ```

    ### Edge `[1, 2]`

    Check:

    ```text
    find(1) != find(2)
    ```

    They are not connected.

    So:

    ```python
    union(1, 2)
    ```

    Now:

    ```text
    {1, 2} {3}
    ```

    Graph:

    ```text
    1 ----- 2
    ```

    ### Edge `[1, 3]`

    Check:

    ```text
    find(1) != find(3)
    ```

    They are not connected.

    So:

    ```python
    union(1, 3)
    ```

    Now:

    ```text
    {1, 2, 3}
    ```

    Graph:

    ```text
    2 ----- 1 ----- 3
    ```

    Notice that `2` and `3` are now already connected through `1`.

    ### Edge `[2, 3]`

    Before performing union, check:

    ```python
    uf.isSame(2, 3)
    ```

    We have:

    ```text
    find(2) == find(3)
    ```

    because both nodes already belong to:

    ```text
    {1, 2, 3}
    ```

    Therefore, adding:

    ```text
    2 ----- 3
    ```

    would produce:

    ```text
        1
       / \
      2---3
    ```

    which contains a cycle.

    So:

    ```text
    [2, 3]
    ```

    is the redundant connection.

    ## Solution

    ```python
    class UnionFind:
        def __init__(self, size):
            self.parent = list(range(size + 1))

        def find(self, node):
            if self.parent[node] != node:
                self.parent[node] = self.find(self.parent[node])
            return self.parent[node]

        def union(self, node1, node2):
            root1 = self.find(node1)
            root2 = self.find(node2)

            if root1 != root2:
                self.parent[root1] = root2

        def isSame(self, node1, node2):
            return self.find(node1) == self.find(node2)

    class Solution:
        def findRedundantConnection(self, edges):
            uf = UnionFind(len(edges))

            for u, v in edges:
                if uf.isSame(u, v):
                    return [u, v]

                uf.union(u, v)
    ```

    ## Why `size + 1`?

    The nodes are numbered:

    ```text
    1 ~ n
    ```

    rather than:

    ```text
    0 ~ n - 1
    ```

    Therefore:

    ```python
    self.parent = list(range(size + 1))
    ```

    allows us to directly access:

    ```text
    parent[1]
    parent[2]
    ...
    parent[n]
    ```

    `parent[0]` is simply unused.

    ## Connection to LeetCode 1971

    LeetCode 1971 asks:

    > Are `source` and `destination` connected?
    >

    So we first union all edges:

    ```python
    for u, v in edges:
        uf.union(u, v)

    return uf.isSame(source, destination)
    ```

    LeetCode 684 asks:

    > Which edge connects two nodes that are already connected?
    >

    So we check **before union**:

    ```python
    for u, v in edges:
        if uf.isSame(u, v):
            return [u, v]

        uf.union(u, v)
    ```

    This is an important Union Find pattern.

    ## Complexity Analysis

    Let `n` be the number of nodes.

    With path compression, `find` and `union` are extremely efficient.

    ### Time Complexity

    ```text
    O(n × α(n))
    ```

    where `α(n)` is the inverse Ackermann function.

    In practice, it grows so slowly that Union Find operations can be considered almost constant time.

    Therefore, the overall complexity is approximately:

    ```text
    O(n)
    ```

    ### Space Complexity

    ```text
    O(n)
    ```

    for the `parent` array.

    ## Key Takeaway

    The most important pattern in this problem is:

    ```text
    For each edge (u, v):

            Are u and v already connected?
                      |
              +-------+-------+
              |               |
             Yes              No
              |               |
         Cycle found       union(u, v)
              |
         return [u, v]
    ```

    In other words:

    > **If two nodes already have the same root before an edge is added, that edge creates a cycle and is therefore redundant.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 25, 2026 |

## Python solution

```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(1 + size))

    def find(self, node):
        if self.parent[node] != node:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]

    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            self.parent[root1] = root2

    def isSame(self, node1, node2):
        return self.find(node1) == self.find(node2)

class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        uf = UnionFind(len(edges))

        for u, v in edges:
            if uf.isSame(u, v):
                return [u, v]

            uf.union(u, v)
```

---

[Back to Day 48 Graph VI](index.md) · [All Algorithm notes](../index.md)
