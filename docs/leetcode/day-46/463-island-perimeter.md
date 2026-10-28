---
hide:
  - toc
---

# 463. Island Perimeter

*Edited by Charles*

**Easy · Python · Graph**

[Original problem](https://leetcode.com/problems/island-perimeter/description/)

!!! tip "Core idea"

    This problem can be solved using **DFS**.

    The key difference between this problem and problems such as **LeetCode 695. Max Area of Island** is what we count.

## Python solution

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        res = 0

        def dfs(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 0:
                return 1

            if grid[i][j] == 2:
                return 0

            grid[i][j] = 2

            return dfs(i - 1, j) + dfs(i + 1, j) + dfs(i, j - 1) + dfs(i, j + 1)

        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    res += dfs(i, j)
                    break

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem can be solved using **DFS**.

    The key difference between this problem and problems such as **LeetCode 695. Max Area of Island** is what we count.

    For island area:

    ```text
    Each land cell contributes 1 to the area.
    ```

    For island perimeter:

    ```text
    Each edge between land and water/outside contributes 1 to the perimeter.
    ```

    Therefore, we start DFS from a land cell and explore its four directions.

    For each direction, there are three possible situations:

    ```text
    1. Outside the grid  → perimeter + 1
    2. Water             → perimeter + 1
    3. Visited land      → perimeter + 0
    4. Unvisited land    → continue DFS
    ```

    ## DFS

    For every land cell, we explore:

    ```text
    Up
    Down
    Left
    Right
    ```

    The DFS returns the perimeter contributed by the current island.

    ## Case 1: Outside the Grid

    If DFS moves outside the grid:

    ```python
    if i < 0 or i >= m or j < 0 or j >= n:
        return 1
    ```

    This means the current land cell has an edge touching the outside of the grid.

    For example:

    ```text
    Outside
       ↑
     ┌───┐
     │ 1 │
     └───┘
    ```

    That edge contributes `1` to the perimeter.

    ## Case 2: Reach Water

    If DFS reaches a water cell:

    ```python
    if grid[i][j] == 0:
        return 1
    ```

    This means there is an edge between land and water:

    ```text
    Land | Water

      1  |  0
         ↑
     perimeter
    ```

    Therefore, this edge also contributes `1`.

    In the code, the outside-grid case and water case can be combined:

    ```python
    if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == 0:
        return 1
    ```

    ## Case 3: Reach Visited Land

    We mark visited land as:

    ```python
    grid[i][j] = 2
    ```

    If DFS reaches a cell with value `2`:

    ```python
    if grid[i][j] == 2:
        return 0
    ```

    Why `0`?

    Because this is an edge between two land cells:

    ```text
    ┌───┬───┐
    │ 1 │ 1 │
    └───┴───┘
         ↑
     internal edge
    ```

    This is an internal edge of the island, so it does **not** contribute to the perimeter.

    ## Case 4: Reach Unvisited Land

    If the current cell is an unvisited land cell:

    ```python
    grid[i][j] == 1
    ```

    mark it as visited:

    ```python
    grid[i][j] = 2
    ```

    Then continue DFS in all four directions:

    ```python
    return (
        dfs(i - 1, j)
        + dfs(i + 1, j)
        + dfs(i, j - 1)
        + dfs(i, j + 1)
    )
    ```

    The sum of these four DFS calls gives the perimeter contributed by the entire connected island.

    ## Hand Simulation

    Consider:

    ```text
    0 0 0
    0 1 0
    0 0 0
    ```

    Start DFS from the center land cell.

    Check its four directions:

    ```text
            0
            ↑
        0 ← 1 → 0
            ↓
            0
    ```

    Every direction reaches water:

    ```text
    Up    → 1
    Down  → 1
    Left  → 1
    Right → 1
    ```

    Therefore:

    ```text
    Perimeter = 1 + 1 + 1 + 1
              = 4
    ```

    ## Another Example

    Consider:

    ```text
    1 1
    ```

    The two land cells share one edge.

    The shared edge is inside the island:

    ```text
    ┌───┬───┐
    │ 1 │ 1 │
    └───┴───┘
         ↑
    shared edge
    ```

    When DFS moves from one land cell to the already visited land cell:

    ```python
    grid[i][j] == 2
    ```

    it returns:

    ```text
    0
    ```

    because the shared edge is not part of the perimeter.

    The remaining six edges touch water or the outside:

    ```text
     ┌───┬───┐
     │ 1   1 │
     └───┴───┘
    ```

    Therefore:

    ```text
    Perimeter = 6
    ```

    ## Why Do We Mark Land as `2`?

    We cannot simply leave visited land as `1`.

    Otherwise, DFS could repeatedly move between neighboring land cells:

    ```text
    A → B → A → B → ...
    ```

    So we mark visited land as:

    ```python
    grid[i][j] = 2
    ```

    Then:

    ```text
    0 = water
    1 = unvisited land
    2 = visited land
    ```

    When DFS reaches `2`, we know that this land cell has already been processed.

    ## Core DFS Logic

    The most important part of this problem can be summarized as:

    ```text
    From a land cell:

        outside grid
            ↓
          return 1

        water
            ↓
          return 1

        visited land
            ↓
          return 0

        unvisited land
            ↓
        mark visited
            ↓
        DFS 4 directions
    ```

    So the perimeter is essentially:

    > **The number of times DFS crosses from land into water or outside the grid.**
    >

    ## Difference from LeetCode 695

    ### LeetCode 695: Max Area of Island

    A land cell itself contributes to the answer:

    ```python
    return 1 + dfs(...) + dfs(...) + dfs(...) + dfs(...)
    ```

    because:

    ```text
    each land cell = area 1
    ```

    ### LeetCode 463: Island Perimeter

    A land cell itself does not directly contribute `1`.

    Instead:

    ```text
    land → water    = perimeter + 1
    land → outside  = perimeter + 1
    land → land     = perimeter + 0
    ```

    Therefore:

    ```python
    return dfs(up) + dfs(down) + dfs(left) + dfs(right)
    ```

    This is the most important difference between the two problems.

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    Each land cell is visited at most once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    In the worst case, the DFS recursion stack may contain `O(m × n)` cells.

    ## Summary

    ```text
    1. Find an unvisited land cell.
    2. Start DFS.
    3. Mark visited land as 2.
    4. If DFS reaches water, return 1.
    5. If DFS goes outside the grid, return 1.
    6. If DFS reaches visited land, return 0.
    7. Add the results from all four directions.
    ```

    The key idea is:

    > **Every time DFS crosses from land to water or outside the grid, we discover one edge of the island's perimeter.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | August 20, 2026 |

---

[Back to Day 46 Graph IV](index.md) · [All LeetCode notes](../index.md)
