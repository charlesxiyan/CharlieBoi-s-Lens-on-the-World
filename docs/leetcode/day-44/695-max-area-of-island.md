---
hide:
  - toc
---

# 695. Max Area of Island

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/max-area-of-island/description/)

!!! tip "Core idea"

    Given a binary matrix `grid`, where

    - `1` represents land,
    - `0` represents water,

## Python solution

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        res = 0

        def dfs(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 0:
                return 0

            grid[i][j] = 0

            return 1 + dfs(i - 1, j) + dfs(i + 1, j) + dfs(i, j - 1) + dfs(i, j + 1)

        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    area = dfs(i, j)
                    res = max(res, area)

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given a binary matrix `grid`, where

    - `1` represents land,
    - `0` represents water,

    return the **maximum area** of an island.

    An island consists of horizontally or vertically connected land cells.

    For example:

    ```python
    grid = [
        [0,0,1,0],
        [1,1,1,0],
        [0,1,0,0],
        [1,0,0,1]
    ]
    ```

    The output is

    ```python
    5
    ```

    The key observation is:

    > Every time we discover an unvisited land cell, we can use **Depth-First Search (DFS)** to visit the entire island and count its area.
    >

    Unlike **LeetCode 200**, which counts the number of islands, this problem asks us to compute the **size of each island** and keep the maximum.

    The DFS starts from one land cell.

    For every visited land cell,

    - count the current cell,
    - recursively explore its four neighboring cells,
    - accumulate the total area.

    Every visited land cell is changed into water (`0`) so that it will never be visited again.

    The recursive function receives the current position.

    ```python
    dfs(i, j)
    ```

    There are three steps.

    ## Step 1: Boundary Check

    If

    - the position is outside the grid, or
    - the current cell is water,

    there is no land to contribute.

    Return

    ```python
    0
    ```

    ```python
    if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 0:
        return 0
    ```

    ## Step 2: Mark the Current Cell as Visited

    Once a land cell is visited,

    change it into water.

    ```python
    grid[i][j] = 0
    ```

    This prevents revisiting the same cell.

    ## Step 3: Compute the Island Area

    The current land cell contributes

    ```python
    1
    ```

    Then recursively add the area from all four directions.

    ```python
    return (
        1
        + dfs(i - 1, j)
        + dfs(i + 1, j)
        + dfs(i, j - 1)
        + dfs(i, j + 1)
    )
    ```

    The returned value is the total area of the current island.

    Traverse every cell in the grid.

    Whenever an unvisited land cell is found,

    ```python
    grid[i][j] == 1
    ```

    run DFS.

    ```python
    area = dfs(i, j)
    ```

    Update the answer.

    ```python
    res = max(res, area)
    ```

    Continue scanning until every island has been processed.

    Consider

    ```python
    grid = [
        [1,1,0],
        [0,1,0],
        [1,0,1]
    ]
    ```

    The grid is

    ```text
    1 1 0
    0 1 0
    1 0 1
    ```

    Initially,

    ```text
    Maximum Area = 0
    ```

    ## Scan Row 0

    The first land cell is

    ```text
    (0,0)
    ```

    Run DFS.

    The connected land cells are

    ```text
    (0,0)
    (0,1)
    (1,1)
    ```

    The recursive calls return

    ```text
    Area = 3
    ```

    Update

    ```text
    Maximum Area = 3
    ```

    The grid becomes

    ```text
    0 0 0
    0 0 0
    1 0 1
    ```

    ## Continue Scanning

    The next land cell is

    ```text
    (2,0)
    ```

    Run DFS.

    Only one land cell exists.

    ```text
    Area = 1
    ```

    The maximum remains

    ```text
    3
    ```

    ## Continue Scanning

    The next land cell is

    ```text
    (2,2)
    ```

    Again,

    ```text
    Area = 1
    ```

    The maximum remains

    ```text
    3
    ```

    Traversal finishes.

    Final answer:

    ```python
    3
    ```

    Unlike LeetCode 200,

    where DFS only marks the island as visited,

    here DFS must also compute the island size.

    Therefore,

    ```python
    dfs(i, j)
    ```

    returns

    ```text
    The total number of land cells
    belonging to the current island.
    ```

    Each recursive call contributes

    ```python
    1
    ```

    for the current cell,

    plus the areas returned by its four neighbors.

    This naturally accumulates the total island area.

    **Time Complexity**

    ```text
    O(m × n)
    ```

    Every cell is visited at most once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    In the worst case, the recursion stack may contain every cell (when the entire grid is one large island).

    **Algorithm**

    ```text
    Depth-First Search (DFS)
    ```

    **State**

    ```text
    Current grid position (i, j)
    ```

    **Visited Mark**

    ```python
    grid[i][j] = 0
    ```

    **Return Value**

    ```text
    Area of the current island
    ```

    **Search Directions**

    ```text
    Up
    Down
    Left
    Right
    ```

    **Main Idea**

    ```text
    Whenever an unvisited land cell is found,
    run DFS to compute the area of the entire island,
    then update the maximum area.
    ```

    ## Common Mistakes

    - Forgetting to mark a land cell as visited, causing the same island to be counted multiple times.
    - Returning nothing from the DFS. In this problem, DFS must return the area contributed by the current island.
    - Forgetting to count the current cell.

    ```python
    return 1 + ...
    ```

    Without the `1`, every island area would be underestimated.

    - Using diagonal directions. Only the four cardinal directions are considered connected.
    - Updating the maximum area inside the DFS. It is clearer to let DFS return the island area first, then update the answer in the outer traversal.
    - Confusing this problem with LeetCode 200. The DFS logic is almost identical, but here the recursion **returns the accumulated area**, whereas in LeetCode 200 it only performs flood fill.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 18, 2026 |

---

[Back to Day 44 Graph II](index.md) · [All Algorithm notes](../index.md)
