---
hide:
  - toc
---

# 1971. Find if Path Exists in Graph

*Edited by Charles*

**Easy · Python · Graph**

[Original problem](https://leetcode.com/problems/find-if-path-exists-in-graph/description/)

!!! tip "Core idea"

    This problem asks whether there exists a valid path from `source` to `destination` in an **undirected graph**.

    We can solve it using **Union Find (Disjoint Set Union / DSU)**.

## Python solution

```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))

    def find(self, node):
        if self.parent[node] != node:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]

    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 == root2:
            return

        self.parent[root1] = root2

    def isSame(self, node1, node2):
        return self.find(node1) == self.find(node2)

class Solution:
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        uf = UnionFind(n)

        for u, v in edges:
            uf.union(u, v)

        return uf.isSame(source, destination)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem asks whether there exists a valid path from `source` to `destination` in an **undirected graph**.

    We can solve it using **Union Find (Disjoint Set Union / DSU)**.

    The key idea is:

    > If two nodes are connected by a path, they will eventually belong to the same set and have the same root.
    >

    Therefore, we can:

    ```text
    1. Initialize every node as its own set.
    2. Traverse all edges.
    3. Union the two nodes of each edge.
    4. Check whether source and destination have the same root.
    ```

    ## Union Find Structure

    We use a `parent` array to represent the sets.

    Initially:

    ```python
    parent = list(range(n))
    ```

    For example, if:

    ```text
    n = 6
    ```

    then:

    ```text
    parent = [0, 1, 2, 3, 4, 5]
    ```

    This means every node initially belongs to its own set:

    ```text
    {0} {1} {2} {3} {4} {5}
    ```

    ## Find

    The `find(x)` function finds the root of node `x`.

    ```python
    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    ```

    The line:

    ```python
    self.parent[x] = self.find(self.parent[x])
    ```

    performs **path compression**.

    For example, suppose:

    ```text
    1 → 2 → 3 → 4
    ```

    After calling:

    ```python
    find(1)
    ```

    the structure can become:

    ```text
    1 ──→ 4
    2 ──→ 4
    3 ──→ 4
    ```

    This makes future `find` operations much faster.

    ## Union

    To connect two nodes `x` and `y`, first find their roots:

    ```python
    root_x = self.find(x)
    root_y = self.find(y)
    ```

    If they already have the same root:

    ```python
    root_x == root_y
    ```

    they are already in the same set.

    Otherwise, connect one root to the other:

    ```python
    self.parent[root_y] = root_x
    ```

    So:

    ```python
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x != root_y:
            self.parent[root_y] = root_x
    ```

    ## Important: Union Does Not Connect Every Node Into One Set

    When we traverse:

    ```python
    for u, v in edges:
        uf.union(u, v)
    ```

    we are **not necessarily connecting every node to one root**.

    We only merge sets when an edge connects them.

    For example:

    ```python
    n = 6

    edges = [
        [0, 1],
        [0, 2],
        [3, 5],
        [5, 4],
        [4, 3]
    ]
    ```

    Initially:

    ```text
    {0} {1} {2} {3} {4} {5}
    ```

    After:

    ```text
    union(0, 1)
    ```

    we have:

    ```text
    {0, 1} {2} {3} {4} {5}
    ```

    After:

    ```text
    union(0, 2)
    ```

    we have:

    ```text
    {0, 1, 2} {3} {4} {5}
    ```

    Then the remaining edges connect `3`, `4`, and `5`:

    ```text
    {0, 1, 2}    {3, 4, 5}
    ```

    So there are still **two separate sets**.

    This correctly represents the two connected components of the graph.

    ## Why Does This Tell Us Whether a Path Exists?

    Consider:

    ```text
    0 ─── 1
    │
    └──── 2

    3 ─── 5
          │
    4 ────┘
    ```

    After processing all edges, Union Find may represent the graph as:

    ```text
          0                 3
         / \               / \
        1   2             5   4
    ```

    Therefore:

    ```text
    find(0) = 0
    find(1) = 0
    find(2) = 0
    ```

    and:

    ```text
    find(3) = 3
    find(4) = 3
    find(5) = 3
    ```

    If:

    ```python
    source = 0
    destination = 2
    ```

    then:

    ```python
    find(0) == find(2)
    ```

    so a path exists.

    But if:

    ```python
    source = 0
    destination = 5
    ```

    then:

    ```python
    find(0) != find(5)
    ```

    so no path exists.

    Therefore:

    ```python
    return uf.is_same(source, destination)
    ```

    ## Why Can We Union Each Edge?

    The graph is **undirected**.

    An edge:

    ```text
    [u, v]
    ```

    means:

    ```text
    u ─── v
    ```

    Therefore, `u` and `v` must belong to the same connected component.

    We can simply call:

    ```python
    uf.union(u, v)
    ```

    Unlike an adjacency list, we do not need to separately store:

    ```text
    u → v
    v → u
    ```

    because Union Find only cares whether the two nodes belong to the same set.

    ## Solution Structure

    We first create a reusable `UnionFind` class:

    ```python
    class UnionFind:
        def __init__(self, n):
            self.parent = list(range(n))

        def find(self, x):
            if x != self.parent[x]:
                self.parent[x] = self.find(self.parent[x])
            return self.parent[x]

        def union(self, x, y):
            root_x = self.find(x)
            root_y = self.find(y)

            if root_x != root_y:
                self.parent[root_y] = root_x

        def is_same(self, x, y):
            return self.find(x) == self.find(y)
    ```

    Then the actual problem becomes very simple:

    ```python
    class Solution:
        def validPath(
            self,
            n: int,
            edges: List[List[int]],
            source: int,
            destination: int
        ) -> bool:

            uf = UnionFind(n)

            for u, v in edges:
                uf.union(u, v)

            return uf.is_same(source, destination)
    ```

    ## Hand Simulation

    Consider:

    ```python
    n = 3
    edges = [
        [0, 1],
        [1, 2],
        [2, 0]
    ]

    source = 0
    destination = 2
    ```

    Initially:

    ```text
    parent = [0, 1, 2]

    Sets:

    {0} {1} {2}
    ```

    ### Process `[0, 1]`

    ```python
    union(0, 1)
    ```

    Roots:

    ```text
    root(0) = 0
    root(1) = 1
    ```

    Merge them:

    ```text
    parent = [0, 0, 2]

    Sets:

    {0, 1} {2}
    ```

    ### Process `[1, 2]`

    ```python
    union(1, 2)
    ```

    First:

    ```text
    find(1) = 0
    find(2) = 2
    ```

    So we are actually merging the two roots:

    ```text
    0 and 2
    ```

    After union:

    ```text
    parent = [0, 0, 0]

    Set:

    {0, 1, 2}
    ```

    ### Process `[2, 0]`

    Both already have the same root:

    ```text
    find(2) = 0
    find(0) = 0
    ```

    Therefore, nothing needs to be changed.

    Finally:

    ```python
    uf.is_same(0, 2)
    ```

    checks:

    ```text
    find(0) == find(2)

    0 == 0
    ```

    Therefore:

    ```text
    True
    ```

    ## Union Find vs DFS/BFS

    This problem can also be solved using DFS or BFS.

    ### DFS / BFS

    The idea is:

    ```text
    Start from source
            ↓
    Traverse connected nodes
            ↓
    Can we reach destination?
    ```

    ### Union Find

    The idea is:

    ```text
    Process every edge
            ↓
    Merge connected nodes into sets
            ↓
    Do source and destination have the same root?
    ```

    So:

    ```text
    DFS / BFS
    → directly search for a path

    Union Find
    → determine whether two nodes belong to the same connected component
    ```

    For this problem, both approaches work.

    ## Complexity Analysis

    Let:

    ```text
    V = number of nodes
    E = number of edges
    ```

    With path compression, Union Find operations are very efficient.

    Processing all edges requires approximately:

    ```text
    O(E × α(V))
    ```

    where `α(V)` is the inverse Ackermann function.

    In practice, `α(V)` is extremely small, so this is almost:

    ```text
    O(E)
    ```

    Space complexity:

    ```text
    O(V)
    ```

    for the `parent` array.

    ## Summary

    ```text
    1. Initialize every node as its own set.

    2. For every edge [u, v]:
           union(u, v)

    3. union() finds the roots of u and v
       and merges their sets.

    4. After processing all edges,
       each connected component has a common root.

    5. Check:
           find(source) == find(destination)

    6. Same root      → path exists
       Different root → path does not exist
    ```

    The most important idea is:

    > **Union Find does not necessarily merge all nodes into one set. It merges exactly the nodes that belong to the same connected component.**
    >

    Therefore:

    > **Two nodes are connected if and only if they have the same root.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 23, 2026 |

---

[Back to Day 47 Graph V](index.md) · [All Algorithm notes](../index.md)
