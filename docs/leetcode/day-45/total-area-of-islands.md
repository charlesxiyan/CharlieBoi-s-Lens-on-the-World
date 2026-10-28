---
hide:
  - toc
---

# Total Area of Islands

*Edited by Charles*

**Medium · Python · Graphs**

!!! tip "Core idea"

    This problem is very similar to **LeetCode 695. Max Area of Island**.

    The main difference is that we only want to count islands that **do not touch the boundary**.

## Python solution

```python
from typing import List

class Solution:
    def closedIslandArea(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        res = 0

        def dfs(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 0:
                return 0

            grid[i][j] = 0
            return 1 + dfs(i - 1, j) + dfs(i + 1, j) + dfs(i, j - 1) + dfs(i, j + 1)

        for i in range(m):
            if grid[i][0] == 1:
                dfs(i, 0)
            if grid[i][n - 1] == 1:
                dfs(i, n - 1)

        for j in range(n):
            if grid[0][j] == 1:
                dfs(0, j)
            if grid[m - 1][j] == 1:
                dfs(m - 1, j)

        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    res += dfs(i, j)

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Description

    You are given an `m x n` binary matrix `grid`, where:

    - `1` represents land.
    - `0` represents water.

    An **island** is a group of land cells connected horizontally or vertically.

    A **closed island** is an island located completely inside the matrix, meaning that **none of its land cells touch any edge of the matrix**.

    The **area** of an island is the total number of land cells that belong to the island.

    Return the **total area of all closed islands**.

    ## Example 1

    **Input:**

    ```python
    grid = [
        [1,1,0,0,0],
        [1,1,0,0,0],
        [0,0,1,0,0],
        [0,0,0,1,1]
    ]
    ```

    **Output:**

    ```text
    1
    ```

    **Explanation:**

    There are three islands in the grid.

    The island in the top-left corner touches the top and left edges of the matrix, so it is not a closed island.

    The island in the bottom-right corner touches the bottom and right edges of the matrix, so it is also not a closed island.

    The single land cell in the center does not touch any edge of the matrix, so it is a closed island with an area of `1`.

    Therefore, the total area of all closed islands is `1`.

    ## Example 2

    **Input:**

    ```python
    grid = [
        [0,0,0,0,0,0],
        [0,1,1,0,1,0],
        [0,1,1,0,1,0],
        [0,0,0,0,0,0]
    ]
    ```

    **Output:**

    ```text
    6
    ```

    **Explanation:**

    There are two closed islands.

    The first closed island contains `4` land cells:

    ```text
    1 1
    1 1
    ```

    The second closed island contains `2` land cells:

    ```text
    1
    1
    ```

    Neither island touches any edge of the matrix.

    Therefore, the total area of all closed islands is:

    ```text
    4 + 2 = 6
    ```

    ## Constraints

    ```text
    m == grid.length
    n == grid[i].length

    1 <= m, n <= 500

    grid[i][j] is either 0 or 1.
    ```

    ## Python



    ## Idea

    This problem is very similar to **LeetCode 695. Max Area of Island**.

    The main difference is that we only want to count islands that **do not touch the boundary**.

    The key idea is:

    > First remove all islands connected to the boundary.
    >
    >
    > After that, every remaining land cell must belong to a closed island.
    >

    Then we can use DFS again to calculate the total area of all remaining islands.

    ## Step 1: DFS

    We use DFS to visit all connected land cells.

    If the current position is:

    - outside the grid, or
    - water (`0`),

    return `0`.

    ```python
    if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 0:
        return 0
    ```

    For a valid land cell, mark it as visited by changing it to `0`.

    ```python
    grid[i][j] = 0
    ```

    Then recursively explore all four directions:

    ```text
    Up
    Down
    Left
    Right
    ```

    The area of the current island is:

    ```python
    1 + dfs(up) + dfs(down) + dfs(left) + dfs(right)
    ```

    The `1` represents the current land cell.

    ## Step 2: Remove All Boundary Islands

    A closed island cannot touch any edge of the grid.

    Therefore, we first perform DFS on every land cell on the four boundaries.

    For the left and right boundaries:

    ```python
    for i in range(m):
        if grid[i][0] == 1:
            dfs(i, 0)
        if grid[i][n - 1] == 1:
            dfs(i, n - 1)
    ```

    For the top and bottom boundaries:

    ```python
    for j in range(n):
        if grid[0][j] == 1:
            dfs(0, j)
        if grid[m - 1][j] == 1:
            dfs(m - 1, j)
    ```

    Since DFS removes the entire connected island, this eliminates every island that touches the boundary.

    ## Step 3: Calculate the Remaining Area

    After removing all boundary-connected islands:

    > Every remaining `1` must belong to a closed island.
    >

    Therefore, traverse the entire grid again.

    Whenever we find a land cell:

    ```python
    if grid[i][j] == 1:
    ```

    run DFS and add its area to the result:

    ```python
    res += dfs(i, j)
    ```

    ## Example

    Consider:

    ```text
    1 1 0 0 0
    1 1 0 0 0
    0 0 1 0 0
    0 0 0 1 1
    ```

    First, remove all islands connected to the boundary.

    The top-left island and bottom-right island are removed:

    ```text
    0 0 0 0 0
    0 0 0 0 0
    0 0 1 0 0
    0 0 0 0 0
    ```

    Now only one closed island remains.

    Its area is:

    ```text
    1
    ```

    Therefore:

    ```text
    Result = 1
    ```

    ## Why This Works

    Any island that touches the boundary is **not** a closed island.

    By starting DFS from every boundary land cell, we remove not only that boundary cell but also the **entire island connected to it**.

    Therefore, after the boundary DFS:

    ```text
    Remaining land = Closed islands
    ```

    This allows us to reuse the same DFS from LeetCode 695 to calculate their areas.

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    Each cell is visited at most once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    In the worst case, the DFS recursion stack may contain `O(m × n)` cells.

    ## Summary

    ```text
    1. DFS from all four boundaries.
    2. Remove every island connected to a boundary.
    3. Traverse the grid again.
    4. Every remaining island is a closed island.
    5. Use DFS to calculate and accumulate their areas.
    ```

    The most important idea is:

    > **Remove boundary-connected islands first, then calculate the area of everything that remains.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 45 Graph III](index.md) · [All LeetCode notes](../index.md)
