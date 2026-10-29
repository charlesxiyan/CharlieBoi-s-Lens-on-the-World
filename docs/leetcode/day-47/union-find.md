---
hide:
  - toc
---

# Union-Find

*Edited by Charles*

**Easy · Python · Graph**

??? question "Problem"

    This is a data-structure study note rather than a single programming problem.

    Maintain a collection of disjoint sets with these operations:

    - `find(x)`: return the representative of the set containing `x`.
    - `union(u, v)`: merge the sets containing `u` and `v`.
    - `is_same(u, v)`: determine whether `u` and `v` belong to the same set.

    Initially, each of the `n` elements belongs to its own set. The note covers
    path compression and union by rank, with examples of connectivity queries.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Union Find, also called **Disjoint Set Union (DSU)**, is a data structure used to manage multiple disjoint sets.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Union Find, also called **Disjoint Set Union (DSU)**, is a data structure used to manage multiple disjoint sets.

    It is especially useful for problems involving:

    ```text
    Connectivity
    Connected Components
    Cycle Detection
    Merging Groups
    ```

    The key operations are:

    ```text
    find(x)     -> find the root of x
    union(u, v) -> merge the sets containing u and v
    is_same(u,v)-> check whether u and v belong to the same set
    ```

    ## Basic Representation

    Instead of explicitly storing every set separately, we use a one-dimensional array:

    ```python
    parent
    ```

    where:

    ```text
    parent[x] = y
    ```

    means that `y` is the parent of `x`.

    For example:

    ```text
    A -> B -> C
    ```

    can be represented as:

    ```python
    parent[A] = B
    parent[B] = C
    parent[C] = C
    ```

    Since:

    ```text
    parent[C] = C
    ```

    `C` is the root of the set.

    Therefore:

    ```text
    find(A) = C
    find(B) = C
    find(C) = C
    ```

    Since they have the same root, they belong to the same set.

    ## Initialization

    Initially, every element belongs to its own set.

    Therefore:

    ```python
    parent[i] = i
    ```

    For example:

    ```text
    0 -> 0
    1 -> 1
    2 -> 2
    3 -> 3
    ```

    In Python:

    ```python
    parent = [i for i in range(n)]
    ```

    or simply:

    ```python
    parent = list(range(n))
    ```

    ## Find

    The `find` operation returns the root of an element.

    Without optimization:

    ```python
    def find(x):
        if x == parent[x]:
            return x

        return find(parent[x])
    ```

    For example:

    ```text
    1 -> 2 -> 3 -> 3
    ```

    Then:

    ```text
    find(1)
    ```

    works like:

    ```text
    find(1)
    → find(2)
    → find(3)
    → 3
    ```

    So the root of `1` is `3`.

    ## Path Compression

    The basic `find` operation may create very deep trees.

    For example:

    ```text
    1 -> 2 -> 3 -> 4 -> 5
    ```

    Finding the root of `1` requires following many parent links.

    Path compression makes every visited node point directly to the root.

    Before:

    ```text
    1 -> 2 -> 3 -> 4 -> 5
    ```

    After:

    ```text
    1 ─┐
    2 ─┤
    3 ─┼──> 5
    4 ─┘
    ```

    Python implementation:

    ```python
    def find(x):
        if x != parent[x]:
            parent[x] = find(parent[x])

        return parent[x]
    ```

    The important line is:

    ```python
    parent[x] = find(parent[x])
    ```

    It both finds the root and updates the parent pointer.

    ## Union

    To merge two elements `u` and `v`, we should not directly connect:

    ```python
    parent[v] = u
    ```

    because `u` and `v` may not be roots.

    Instead, first find their roots:

    ```python
    root_u = find(u)
    root_v = find(v)
    ```

    If:

    ```python
    root_u == root_v
    ```

    they already belong to the same set.

    Otherwise, connect one root to the other.

    Basic version:

    ```python
    def union(u, v):
        root_u = find(u)
        root_v = find(v)

        if root_u == root_v:
            return

        parent[root_v] = root_u
    ```

    ## Check Whether Two Elements Are Connected

    Two elements belong to the same set if they have the same root.

    ```python
    def is_same(u, v):
        return find(u) == find(v)
    ```

    For example:

    ```text
    1 -> 3
    2 -> 3
    3 -> 3
    ```

    Then:

    ```python
    find(1) == find(2)
    ```

    so:

    ```python
    is_same(1, 2) == True
    ```

    Path compression optimizes `find`.

    Another optimization is **Union by Rank**.

    The idea is:

    > Always attach the shorter tree under the taller tree.
    >

    This prevents the tree from becoming unnecessarily deep.

    We maintain another array:

    ```python
    rank
    ```

    where `rank[x]` represents an approximate height of the tree rooted at `x`.

    Initialization:

    ```python
    rank = [1] * n
    ```

    ## Union by Rank Logic

    Suppose:

    ```text
    rank[root_u] < rank[root_v]
    ```

    Then attach `root_u` under `root_v`:

    ```python
    parent[root_u] = root_v
    ```

    If:

    ```text
    rank[root_u] > rank[root_v]
    ```

    attach `root_v` under `root_u`.

    If the ranks are equal, either root can become the new root.

    For example:

    ```python
    parent[root_v] = root_u
    rank[root_u] += 1
    ```

    ## Python Implementation

    ```python
    class UnionFind:

        def __init__(self, n):
            self.parent = list(range(n))
            self.rank = [1] * n

        def find(self, x):
            if x != self.parent[x]:
                self.parent[x] = self.find(self.parent[x])

            return self.parent[x]

        def union(self, u, v):
            root_u = self.find(u)
            root_v = self.find(v)

            if root_u == root_v:
                return False

            if self.rank[root_u] < self.rank[root_v]:
                self.parent[root_u] = root_v

            elif self.rank[root_u] > self.rank[root_v]:
                self.parent[root_v] = root_u

            else:
                self.parent[root_v] = root_u
                self.rank[root_u] += 1

            return True

        def is_same(self, u, v):
            return self.find(u) == self.find(v)
    ```

    ## Example

    Suppose:

    ```python
    uf = UnionFind(6)
    ```

    Initially:

    ```text
    0   1   2   3   4   5
    │   │   │   │   │   │
    0   1   2   3   4   5
    ```

    Each element belongs to its own set.

    ### Union 1 and 2

    ```python
    uf.union(1, 2)
    ```

    Possible structure:

    ```text
    1
    |
    2
    ```

    Now:

    ```python
    uf.is_same(1, 2)
    ```

    returns:

    ```python
    True
    ```

    ### Union 2 and 3

    ```python
    uf.union(2, 3)
    ```

    Since:

    ```python
    find(2) = 1
    ```

    we actually merge:

    ```text
    root(2) = 1
    root(3) = 3
    ```

    Possible result:

    ```text
        1
       / \
      2   3
    ```

    Now:

    ```python
    find(1) = 1
    find(2) = 1
    find(3) = 1
    ```

    Therefore:

    ```python
    uf.is_same(1, 3)
    ```

    returns:

    ```python
    True
    ```

    ## Why Do We Union the Roots?

    Suppose:

    ```text
    1 -> 2 -> 3
    ```

    and:

    ```text
    4 -> 5
    ```

    If we want to merge `1` and `4`, we should first find:

    ```text
    find(1) = 3
    find(4) = 5
    ```

    Then connect:

    ```text
    3 -> 5
    ```

    or:

    ```text
    5 -> 3
    ```

    This merges the entire two sets.

    If we only write:

    ```python
    parent[4] = 1
    ```

    we may not correctly connect the two tree structures.

    Therefore:

    > Always find the roots before performing union.
    >

    ## Why Does Union Find Work?

    Each connected component is represented by one tree.

    All elements in the same set eventually point to the same root.

    Therefore:

    ```text
    Same root
        ↓
    Same set
    ```

    Different roots mean:

    ```text
    Different sets
    ```

    The root acts like the ID of the connected component.

    ## Time Complexity

    With both:

    ```text
    Path Compression
    +
    Union by Rank
    ```

    the amortized time complexity of `find` and `union` is approximately:

    ```text
    O(α(n))
    ```

    where `α(n)` is the inverse Ackermann function.

    For all practical input sizes:

    ```text
    α(n) ≈ constant
    ```

    So Union Find operations are effectively very close to:

    ```text
    O(1)
    ```

    ## Summary

    ### Initialization

    ```python
    parent = list(range(n))
    rank = [1] * n
    ```

    Each element starts as its own root.

    ### Find

    ```python
    def find(x):
        if x != parent[x]:
            parent[x] = find(parent[x])

        return parent[x]
    ```

    Use **path compression** to make future searches faster.

    ### Union

    ```text
    1. Find the roots of u and v.
    2. If the roots are equal, they are already connected.
    3. Otherwise, merge the two roots.
    4. Attach the lower-rank tree under the higher-rank tree.
    ```

    ### Check Connectivity

    ```python
    find(u) == find(v)
    ```

    means that `u` and `v` belong to the same set.

    ## Common Mistakes

    - Do not directly connect `u` and `v` without finding their roots first.
    - Do not forget initialization:

    ```python
    parent[i] = i
    ```

    Every element must initially be its own root.

    - Path compression happens inside `find`:

    ```python
    parent[x] = find(parent[x])
    ```

    - Union by rank should compare the **roots' ranks**, not the original nodes' ranks.
    - If two nodes already have the same root, do not merge them again.
    - Union Find is mainly designed for **undirected connectivity** problems. It is not generally a replacement for DFS/BFS in arbitrary directed reachability problems.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 23, 2026 |

## Python solution

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x == root_y:
            return False

        if self.rank[root_x] < self.rank[root_y]:
            root_x, root_y = root_y, root_x

        self.parent[root_y] = root_x

        if self.rank[root_x] == self.rank[root_y]:
            self.rank[root_x] += 1

        return True

    def is_same(self, x, y):
        return self.find(x) == self.find(y)
```

---

[Back to Day 47 Graph V](index.md) · [All Algorithm notes](../index.md)
