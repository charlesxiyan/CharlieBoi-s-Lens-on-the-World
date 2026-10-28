---
hide:
  - toc
---

# 685. Redundant Connection II

*Edited by Charles*

**Hard · Python · Graph**

[Original problem](https://leetcode.com/problems/redundant-connection-ii/)

!!! tip "Core idea"

    This problem is an extension of **LeetCode 684. Redundant Connection**.

    In LeetCode 684, the graph is undirected, so the main problem is detecting a **cycle**.

## Python solution

```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(1 + size))

    def find(self, node):
        if node != self.parent[node]:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]

    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            self.parent[root2] = root1

    def isSame(self, node1, node2):
        return self.find(node1) == self.find(node2)

class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        parent = [0] * (len(edges) + 1)
        candidate1 = None
        candidate2 = None

        for u, v in edges:
            if parent[v] == 0:
                parent[v] = u
            else:
                candidate1 = [parent[v], v]
                candidate2 = [u, v]
                break

        uf = UnionFind(len(edges))

        for u, v in edges:
            if candidate2 == [u, v]:
                continue
            if uf.isSame(u, v):
                if not candidate1:
                    return [u, v]
                return candidate1
            uf.union(u, v)

        return candidate2
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem is an extension of **LeetCode 684. Redundant Connection**.

    In LeetCode 684, the graph is undirected, so the main problem is detecting a **cycle**.

    In this problem, the graph is directed and should originally be a **rooted tree**.

    A valid rooted tree has two important properties:

    - Every node except the root has exactly one parent.
    - There is no cycle.

    The graph originally contains `n` nodes and `n - 1` edges, and then **exactly one extra directed edge** is added.

    Therefore, the extra edge can cause two types of problems:

    ```text
    1. A node has two parents.
    2. A cycle is created.
    ```

    These two problems can also happen at the same time.

    So there are three possible cases:

    ```text
    Case 1: Cycle only

    Case 2: Two parents, but no cycle

    Case 3: Two parents + cycle
    ```

    We first use an array:

    ```python
    parent = [0] * (n + 1)
    ```

    Here, this `parent` represents the **actual directed parent relationship** in the graph.

    For every directed edge:

    ```text
    u → v
    ```

    we check whether `v` already has a parent.

    If:

    ```python
    parent[v] == 0
    ```

    then `v` does not have a parent yet:

    ```python
    parent[v] = u
    ```

    Otherwise, `v` already has a parent, which means we found a node with **two parents**.

    Suppose:

    ```text
    parent[v] = x
    ```

    and now we encounter:

    ```text
    u → v
    ```

    Then the two candidate edges are:

    ```python
    candidate1 = [x, v]
    candidate2 = [u, v]
    ```

    where:

    ```text
    candidate1 = the earlier edge
    candidate2 = the later edge
    ```

    For example:

    ```text
    1 → 3
    2 → 3
    ```

    When processing `[2, 3]`, we discover that `3` already has parent `1`.

    Therefore:

    ```python
    candidate1 = [1, 3]
    candidate2 = [2, 3]
    ```

    If a node has two parents, one of these two incoming edges must be removed.

    So instead of checking every edge in the graph, we only need to determine:

    ```text
    Should we remove candidate1?

    or

    Should we remove candidate2?
    ```

    We can determine this using Union Find.

    If there is a two-parent situation, we first assume that the **later edge** is the redundant one.

    Therefore, when processing the graph with Union Find, we temporarily skip:

    ```python
    candidate2
    ```

    ```python
    if [u, v] == candidate2:
        continue
    ```

    If `candidate2` is `None`, the comparison:

    ```python
    [u, v] == None
    ```

    is simply `False`, so no edge will be skipped.

    We use the same Union Find idea as LeetCode 684.

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
    ```

    For each edge:

    ```text
    u → v
    ```

    if:

    ```python
    uf.isSame(u, v)
    ```

    is already `True`, then `u` and `v` are already connected.

    Adding this edge creates a cycle.

    ## Case 1: Cycle Only

    Example:

    ```text
    1 → 2
    ↑   ↓
    └── 3
    ```

    Edges:

    ```python
    [[1, 2], [2, 3], [3, 1]]
    ```

    There is no node with two parents.

    Therefore:

    ```text
    candidate1 = None
    candidate2 = None
    ```

    During Union Find, eventually we process:

    ```text
    3 → 1
    ```

    At this point:

    ```python
    uf.isSame(3, 1)
    ```

    is already `True`.

    So this edge creates the cycle.

    We directly return:

    ```python
    [3, 1]
    ```

    In code:

    ```python
    if uf.isSame(u, v):
        if candidate1 is None:
            return [u, v]
    ```

    ## Case 2: Two Parents, No Cycle

    Example:

    ```text
        1
       / \
      ↓   ↓
      2 → 3
    ```

    Edges:

    ```python
    [[1, 2], [1, 3], [2, 3]]
    ```

    Node `3` has two parents:

    ```text
    1 → 3
    2 → 3
    ```

    Therefore:

    ```python
    candidate1 = [1, 3]
    candidate2 = [2, 3]
    ```

    Now temporarily skip:

    ```text
    2 → 3
    ```

    The remaining graph is:

    ```text
        1
       / \
      ↓   ↓
      2   3
    ```

    There is no cycle.

    Therefore, removing `candidate2` successfully restores a valid rooted tree.

    So:

    ```python
    return candidate2
    ```

    Answer:

    ```python
    [2, 3]
    ```

    ## Case 3: Two Parents + Cycle

    Consider:

    ```python
    edges = [
        [2, 1],
        [3, 1],
        [4, 2],
        [1, 4]
    ]
    ```

    Node `1` has two parents:

    ```text
    2 → 1
    3 → 1
    ```

    Therefore:

    ```python
    candidate1 = [2, 1]
    candidate2 = [3, 1]
    ```

    We first temporarily skip:

    ```text
    3 → 1
    ```

    But the remaining graph contains:

    ```text
    2 → 1 → 4 → 2
    ```

    There is still a cycle.

    This means:

    > Removing `candidate2` does not solve the problem.
    >

    Therefore, the edge that must be removed is:

    ```python
    candidate1
    ```

    So:

    ```python
    return candidate1
    ```

    The entire problem can be summarized as:

    ```text
                    Scan all edges
                          |
                          v
               Does a node have
                 two parents?
                  /       \
                No         Yes
                |           |
                |     candidate1
                |     candidate2
                |           |
                |     Skip candidate2
                |           |
                v           v
             Union Find  Union Find
                |           |
                v           v
              Cycle?      Cycle?
                |        /      \
                |      Yes       No
                v       |         |
           return edge  |         |
                        v         v
                  candidate1  candidate2
    ```

    Therefore:

    ```text
    Case 1:
    No two-parent node + cycle
    → return the cycle-producing edge

    Case 2:
    Two-parent node + no cycle after skipping candidate2
    → return candidate2

    Case 3:
    Two-parent node + cycle still exists after skipping candidate2
    → return candidate1
    ```

    ```python
    from typing import List

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
        def findRedundantDirectedConnection(
            self, edges: List[List[int]]
        ) -> List[int]:

            n = len(edges)

            parent = [0] * (n + 1)
            candidate1 = None
            candidate2 = None

            # Find whether a node has two parents
            for u, v in edges:
                if parent[v] == 0:
                    parent[v] = u
                else:
                    candidate1 = [parent[v], v]
                    candidate2 = [u, v]
                    break

            uf = UnionFind(n)

            # Detect cycle while skipping candidate2
            for u, v in edges:

    						# Skip the last redundant edge
                if [u, v] == candidate2:
                    continue

                if uf.isSame(u, v):

                    # Case 1: cycle only
                    if candidate1 is None:
                        return [u, v]

                    # Case 3: two parents + cycle
                    return candidate1

                uf.union(u, v)

            # Case 2: two parents but no cycle
            return candidate2
    ```

    There are two different concepts called `parent` in this solution.

    They should not be confused.

    ## Directed Graph Parent

    ```python
    parent = [0] * (n + 1)
    ```

    This represents:

    ```text
    parent[v] = u

    means

    u → v
    ```

    Its purpose is to detect:

    ```text
    Does a node have two parents?
    ```

    ## Union Find Parent

    Inside:

    ```python
    uf.parent
    ```

    the meaning is different.

    It represents the internal structure of the Disjoint Set Union.

    Its purpose is to determine:

    ```text
    Are two nodes already connected?
    ```

    So:

    ```text
    Graph parent
    → detect two parents

    Union Find parent
    → detect connectivity / cycle
    ```

    The graph originally is a rooted tree.

    A tree with `n` nodes has:

    ```text
    n - 1 edges
    ```

    and contains no cycle.

    The problem adds exactly **one extra edge**.

    Therefore, if the extra edge creates a cycle, that cycle consists of:

    ```text
    the newly added edge
    +
    the unique path that already existed
    between its two endpoints
    ```

    A tree has exactly one path between any two connected nodes.

    Therefore, one extra edge can introduce at most one cycle.

    However, the extra edge does **not necessarily create a cycle**.

    It may instead create a node with two parents.

    LeetCode 684 only needs to detect:

    ```text
    Cycle
    ```

    So:

    ```python
    for u, v in edges:
        if uf.isSame(u, v):
            return [u, v]

        uf.union(u, v)
    ```

    LeetCode 685 must handle:

    ```text
    Cycle
    +
    Two Parents
    ```

    Therefore, we first detect the two-parent situation and then use the same Union Find cycle detection from LeetCode 684.

    A useful way to remember the relationship is:

    ```text
    LeetCode 684
        |
        |  Cycle Detection
        v
    Union Find

    LeetCode 685
        |
        +-- Detect Two Parents
        |
        +-- Use 684's Union Find
            to Detect Cycle
    ```

    Let `n` be the number of nodes.

    We scan the edges once to detect a node with two parents:

    ```text
    O(n)
    ```

    Then we scan the edges again with Union Find:

    ```text
    O(n × α(n))
    ```

    where `α(n)` is the inverse Ackermann function and is effectively constant for practical input sizes.

    Therefore, the overall time complexity is approximately:

    ```text
    O(n)
    ```

    Space complexity:

    ```text
    O(n)
    ```

    for the graph-parent array and the Union Find structure.

    The most important thing to remember is that LeetCode 685 has **three cases**:

    ```text
    1. Cycle only
       → return the edge that creates the cycle

    2. Two parents, no cycle after skipping the later edge
       → return the later parent edge

    3. Two parents, cycle still exists after skipping the later edge
       → return the earlier parent edge
    ```

    A compact way to remember the algorithm is:

    ```text
    Find two parents first
            ↓
    candidate1 = earlier edge
    candidate2 = later edge
            ↓
    Skip candidate2
            ↓
    Run Union Find
            ↓
        Is there a cycle?
          /          \
        Yes           No
         |             |
    candidate1     candidate2
    ```

    If there is **no two-parent situation**, simply use Union Find exactly like LeetCode 684 and return the edge that creates the cycle.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 25, 2026 |

---

[Back to Day 48 Graph VI](index.md) · [All LeetCode notes](../index.md)
