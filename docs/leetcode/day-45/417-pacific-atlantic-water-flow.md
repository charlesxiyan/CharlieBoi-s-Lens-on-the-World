---
hide:
  - toc
---

# 417. Pacific Atlantic Water Flow

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/pacific-atlantic-water-flow/)

!!! tip "Core idea"

    We need to find all cells where water can flow to **both the Pacific Ocean and the Atlantic Ocean**.

## Python solution

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        m, n = len(heights), len(heights[0])
        res = []

        pacific = [[False] * n for _ in range(m)]
        atlantic = [[False] * n for _ in range(m)]

        def dfs(i, j, ocean):
            ocean[i][j] = True
            directions = [(i - 1, j), (i + 1, j), (i, j - 1), (i, j + 1)]
            for p, q in directions:
                if 0 <= p < m and 0 <= q < n and not ocean[p][q] and heights[p][q] >= heights[i][j]:
                    dfs(p, q, ocean)

        for i in range(m):
            dfs(i, 0, pacific)
            dfs(i, n - 1, atlantic)

        for j in range(n):
            dfs(0, j, pacific)
            dfs(m - 1, j, atlantic)

        for i in range(m):
            for j in range(n):
                if pacific[i][j] and atlantic[i][j]:
                    res.append([i, j])

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    We need to find all cells where water can flow to **both the Pacific Ocean and the Atlantic Ocean**.

    Water can normally flow from a cell to a neighboring cell if the neighboring height is **less than or equal to** the current height:

    ```text
    Higher → Lower
    ```

    A brute-force approach would start from every cell and check whether water can eventually reach both oceans.

    Instead, we can reverse the problem:

    > Start from each ocean and search backward to find all cells that can reach that ocean.
    >

    This is similar to LeetCode 1254, where we also start searching from the boundaries instead of checking every region individually.

    ## Reverse DFS

    Normally, water can flow:

    ```text
    High → Low
    ```

    If we reverse the direction and start from the ocean, we can move:

    ```text
    Low → High
    ```

    Therefore, from the current cell `(i, j)`, we can visit a neighboring cell `(x, y)` if:

    ```python
    heights[x][y] >= heights[i][j]
    ```

    Why?

    Suppose:

    ```text
    A = 5
    B = 3
    ```

    Water can normally flow:

    ```text
    5 → 3 → Ocean
    ```

    So if we start from the ocean and have already reached height `3`, we should be allowed to reverse the path:

    ```text
    Ocean → 3 → 5
    ```

    Therefore, during reverse DFS, we can only move to cells with the **same or greater height**.

    ## Step 1: Pacific Ocean

    The Pacific Ocean touches:

    ```text
    Top boundary
    Left boundary
    ```

    Create a visited matrix:

    ```python
    pacific = [[False] * n for _ in range(m)]
    ```

    Then start DFS from every cell on the top and left boundaries.

    ```python
    for i in range(m):
        dfs(i, 0, pacific)

    for j in range(n):
        dfs(0, j, pacific)
    ```

    After these searches:

    ```text
    pacific[i][j] = True
    ```

    means that water from `(i, j)` can flow to the Pacific Ocean.

    ## Step 2: Atlantic Ocean

    The Atlantic Ocean touches:

    ```text
    Bottom boundary
    Right boundary
    ```

    Create another visited matrix:

    ```python
    atlantic = [[False] * n for _ in range(m)]
    ```

    Then start DFS from the bottom and right boundaries.

    ```python
    for i in range(m):
        dfs(i, n - 1, atlantic)

    for j in range(n):
        dfs(m - 1, j, atlantic)
    ```

    After these searches:

    ```text
    atlantic[i][j] = True
    ```

    means that water from `(i, j)` can flow to the Atlantic Ocean.

    ## Step 3: Find the Intersection

    Now we have two sets of reachable cells:

    ```text
    Pacific reachable cells

    Atlantic reachable cells
    ```

    A cell can reach both oceans if:

    ```python
    pacific[i][j] and atlantic[i][j]
    ```

    Therefore:

    ```python
    for i in range(m):
        for j in range(n):
            if pacific[i][j] and atlantic[i][j]:
                res.append([i, j])
    ```

    ## Hand Simulation

    Consider:

    ```text
    1  2  2
    3  2  3
    2  4  5
    ```

    The oceans are located around the grid like this:

    ```text
            Pacific
         ↓   ↓   ↓
        -------------
     → | 1 | 2 | 2 |
     P |---|---|---| ← Atlantic
     a | 3 | 2 | 3 |
     c |---|---|---| ← Atlantic
     → | 2 | 4 | 5 |
        -------------
          ↑   ↑   ↑
           Atlantic
    ```

    ### Pacific Search

    Start from the:

    ```text
    Top row
    Left column
    ```

    and move toward cells with the same or greater height.

    For example:

    ```text
    1 → 2
    ```

    is allowed during reverse DFS because:

    ```text
    2 >= 1
    ```

    This means that in the normal water-flow direction:

    ```text
    2 → 1 → Pacific
    ```

    is possible.

    ### Atlantic Search

    Start from the:

    ```text
    Bottom row
    Right column
    ```

    and perform the same reverse DFS.

    Again, we only move to neighboring cells whose height is greater than or equal to the current height.

    ### Final Result

    After both searches, we check:

    ```text
    Pacific reachable
            ∩
    Atlantic reachable
    ```

    Any cell appearing in both visited matrices can send water to both oceans.

    ## Why Not Start DFS From Every Cell?

    A more direct approach would be:

    ```text
    For every cell:
        DFS toward lower cells
        Check whether Pacific can be reached
        Check whether Atlantic can be reached
    ```

    This repeats a large amount of work.

    Instead, we reverse the search:

    ```text
    Pacific boundaries
            ↓
       Reverse DFS
            ↓
    All cells that can reach Pacific

    Atlantic boundaries
            ↓
       Reverse DFS
            ↓
    All cells that can reach Atlantic
    ```

    Then simply take the intersection.

    This is the key optimization.

    ## Connection to LeetCode 1254

    The two problems use a similar idea:

    ### LeetCode 1254

    Instead of checking whether every island touches the boundary:

    ```text
    Start from boundary
            ↓
    Remove boundary-connected islands
            ↓
    Count what remains
    ```

    ### LeetCode 417

    Instead of checking whether every cell can reach an ocean:

    ```text
    Start from ocean boundaries
            ↓
    Reverse DFS
            ↓
    Find all cells that can reach the ocean
    ```

    The common pattern is:

    > **Start from the boundary and reverse the problem.**
    >

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    Each cell can be visited at most once during the Pacific DFS and once during the Atlantic DFS.

    Therefore:

    ```text
    O(m × n) + O(m × n) = O(m × n)
    ```

    **Space Complexity**

    ```text
    O(m × n)
    ```

    We use two visited matrices:

    ```text
    pacific
    atlantic
    ```

    The DFS recursion stack may also require `O(m × n)` space in the worst case.

    ## Summary

    ```text
    1. Water normally flows from high → low.
    2. Reverse the problem and search from the oceans.
    3. Reverse DFS moves from low → high.
    4. Start Pacific DFS from top + left boundaries.
    5. Start Atlantic DFS from bottom + right boundaries.
    6. Record reachable cells in two visited matrices.
    7. Take the intersection of the two matrices.
    ```

    The most important idea is:

    > **Do not start from every cell and search for the oceans. Start from both oceans and search backward.**
    >

    And the most important DFS condition is:

    ```python
    heights[next_i][next_j] >= heights[i][j]
    ```

    because we are performing the water flow process **in reverse**.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | August 19, 2026 |

---

[Back to Day 45 Graph III](index.md) · [All LeetCode notes](../index.md)
