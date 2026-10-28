---
hide:
  - toc
---

# 1254. Number of Closed Islands

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/number-of-closed-islands/description/)

!!! tip "Core idea"

    This problem is similar to **LeetCode 200. Number of Islands**, but there are two important differences:

    Also, we only want to count **closed islands**.

## Python solution

```python
class Solution:
    def closedIsland(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        res = 0

        def dfs(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 1:
                return
            grid[i][j] = 1
            dfs(i - 1, j)
            dfs(i + 1, j)
            dfs(i, j - 1)
            dfs(i, j + 1)

        for i in range(m):
            if grid[i][0] == 0:
                dfs(i, 0)
            if grid[i][n - 1] == 0:
                dfs(i, n - 1)

        for j in range(n):
            if grid[0][j] == 0:
                dfs(0, j)
            if grid[m - 1][j] == 0:
                dfs(m - 1, j)

        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    dfs(i, j)
                    res += 1

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem is similar to **LeetCode 200. Number of Islands**, but there are two important differences:

    ```text
    LeetCode 200:
    1 = land
    0 = water

    LeetCode 1254:
    0 = land
    1 = water
    ```

    Also, we only want to count **closed islands**.

    A closed island is an island that is completely surrounded by water and **does not touch any boundary of the grid**.

    The key idea is:

    > First remove all islands connected to the boundary.
    >
    >
    > Then every remaining island must be a closed island.
    >

    ## Step 1: DFS

    Use DFS to visit all connected land cells.

    If the current position is:

    - outside the grid, or
    - water (`1`),

    stop the DFS.

    ```python
    if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 1:
        return
    ```

    When we visit a land cell (`0`), mark it as water:

    ```python
    grid[i][j] = 1
    ```

    This means the cell has already been visited.

    Then explore all four directions:

    ```python
    dfs(i - 1, j)
    dfs(i + 1, j)
    dfs(i, j - 1)
    dfs(i, j + 1)
    ```

    ## Step 2: Remove All Boundary Islands

    Any island that touches the boundary cannot be a closed island.

    Therefore, before counting islands, perform DFS on every land cell on the four boundaries.

    For the left and right boundaries:

    ```python
    for i in range(m):
        if grid[i][0] == 0:
            dfs(i, 0)

        if grid[i][n - 1] == 0:
            dfs(i, n - 1)
    ```

    For the top and bottom boundaries:

    ```python
    for j in range(n):
        if grid[0][j] == 0:
            dfs(0, j)

        if grid[m - 1][j] == 0:
            dfs(m - 1, j)
    ```

    DFS will remove not only the boundary land cell itself, but also the **entire island connected to it**.

    Therefore, after this step:

    ```text
    All non-closed islands → removed
    All remaining land     → closed islands
    ```

    ## Step 3: Count the Remaining Islands

    Now traverse the entire grid again.

    Whenever we find:

    ```python
    grid[i][j] == 0
    ```

    we have discovered a new closed island.

    So:

    ```python
    res += 1
    ```

    Then run DFS:

    ```python
    dfs(i, j)
    ```

    to remove the entire closed island.

    This ensures that each closed island is counted exactly once.

    ## Hand Simulation

    Consider:

    ```text
    1 1 1 1 1
    1 0 0 1 0
    1 0 1 1 0
    1 1 0 0 1
    1 1 1 1 1
    ```

    Remember:

    ```text
    0 = land
    1 = water
    ```

    ### Remove Boundary Islands

    The `0`s on the right boundary are connected to the boundary:

    ```text
    1 1 1 1 1
    1 0 0 1 0  ← boundary land
    1 0 1 1 0  ← connected
    1 1 0 0 1
    1 1 1 1 1
    ```

    DFS removes them.

    After removing all boundary-connected land:

    ```text
    1 1 1 1 1
    1 0 0 1 1
    1 0 1 1 1
    1 1 0 0 1
    1 1 1 1 1
    ```

    Now every remaining `0` belongs to a closed island.

    There are two connected components:

    ```text
    0 0
    0
    ```

    and

    ```text
    0 0
    ```

    Therefore:

    ```text
    Result = 2
    ```

    ## Why Do We Change `0` to `1`?

    In this problem:

    ```text
    0 = land
    1 = water
    ```

    When a land cell has been visited, we change:

    ```python
    grid[i][j] = 1
    ```

    This prevents DFS from visiting the same cell again.

    It is equivalent to using a separate `visited` array, but requires no additional matrix.

    ## Why Remove Boundary Islands First?

    This is the most important idea of the problem.

    Instead of checking every island and asking:

    ```text
    "Does this island touch the boundary?"
    ```

    we reverse the logic:

    ```text
    Start from the boundary
            ↓
    Remove everything connected to the boundary
            ↓
    Whatever remains must be closed
    ```

    This makes the problem much simpler.

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
    1. Remember that 0 = land and 1 = water.
    2. Use DFS to flood land by changing 0 → 1.
    3. DFS from all four boundaries first.
    4. This removes every island that is not closed.
    5. Traverse the grid again.
    6. Every remaining 0 represents an unvisited closed island.
    7. Increase res by 1 and DFS the entire island.
    ```

    The key idea is:

    > **Remove all boundary-connected islands first, then count the islands that remain.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 19, 2026 |

---

[Back to Day 45 Graph III](index.md) · [All LeetCode notes](../index.md)
