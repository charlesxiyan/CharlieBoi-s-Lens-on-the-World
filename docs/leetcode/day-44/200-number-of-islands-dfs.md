---
hide:
  - toc
---

# 200. Number of Islands (DFS)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/number-of-islands/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given a 2D grid consisting of `'1'` (land) and `'0'` (water), count the number of islands.

        An island is formed by **horizontally or vertically adjacent** land cells. The grid is completely surrounded by water.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given a 2D grid consisting of `'1'` (land) and `'0'` (water), count the number of islands.

    An island is formed by **horizontally or vertically adjacent** land cells. The grid is completely surrounded by water.

    For example:

    ```python
    grid = [
        ["1","1","0","0","0"],
        ["1","1","0","0","0"],
        ["0","0","1","0","0"],
        ["0","0","0","1","1"]
    ]
    ```

    Output:

    ```python
    3
    ```

    The key observation is:

    > Every time we discover an unvisited land cell (`'1'`), we have found a **new island**.
    >

    We can then use **Depth-First Search (DFS)** to visit every connected land cell and mark it as visited.

    The DFS starts from one land cell and explores all connected land cells in four directions:

    - Up
    - Down
    - Left
    - Right

    Every visited land cell is marked as water (`'0'`) so it will never be counted again.

    Thus,

    - Every island contributes exactly **one** to the final answer.
    - Every land cell is visited exactly once.

    The recursive function receives the current position.

    ```python
    dfs(i, j)
    ```

    There are two steps.

    ## Step 1: Boundary Check

    Stop the recursion if

    - the position is outside the grid, or
    - the current cell is already water.

    ```python
    if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == '0':
        return
    ```

    ## Step 2: Mark as Visited

    Once a land cell is visited,

    change it into water.

    ```python
    grid[i][j] = '0'
    ```

    This prevents revisiting the same cell.

    ## Step 3: Explore Four Directions

    Continue searching all neighboring cells.

    ```python
    dfs(i - 1, j)
    dfs(i + 1, j)
    dfs(i, j - 1)
    dfs(i, j + 1)
    ```

    Eventually, every connected land cell belonging to the same island will be visited.

    Traverse every cell in the grid.

    Whenever an unvisited land cell is found,

    ```python
    grid[i][j] == '1'
    ```

    we have discovered a **new island**.

    Increase the answer by one.

    ```python
    res += 1
    ```

    Then use DFS to mark the entire island as visited.

    ```python
    dfs(i, j)
    ```

    Consider

    ```python
    grid = [
        ["1","1","0"],
        ["0","1","0"],
        ["1","0","1"]
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
    Result = 0
    ```

    ## Scan Row 0

    The first land cell is

    ```text
    (0,0)
    ```

    Increase the answer.

    ```text
    Result = 1
    ```

    Run DFS.

    The connected land cells are

    ```text
    (0,0)
    (0,1)
    (1,1)
    ```

    After DFS,

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

    This is a new island.

    ```text
    Result = 2
    ```

    DFS changes it into

    ```text
    0
    ```

    Now the grid becomes

    ```text
    0 0 0
    0 0 0
    0 0 1
    ```

    ## Continue Scanning

    The next land cell is

    ```text
    (2,2)
    ```

    Another new island.

    ```text
    Result = 3
    ```

    DFS visits this single cell.

    The entire grid becomes

    ```text
    0 0 0
    0 0 0
    0 0 0
    ```

    Traversal finishes.

    Final answer:

    ```python
    3
    ```

    Without marking visited cells,

    the same land cell could be reached multiple times from different directions.

    For example,

    ```text
    1 1
    1 1
    ```

    Starting from the top-left cell,

    the DFS would repeatedly revisit already explored cells.

    By changing

    ```python
    '1' -> '0'
    ```

    every land cell is visited only once.

    This also removes the need for an additional `visited` array.

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
    grid[i][j] = '0'
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
    Every time an unvisited land cell is found,
    it represents a new island.
    Use DFS to flood the entire island,
    then continue scanning the grid.
    ```

    ## Common Mistakes

    - Forgetting to mark visited land cells. This causes the same island to be counted multiple times.
    - Using diagonal directions. Only the four cardinal directions (up, down, left, right) are considered connected.
    - Incrementing the answer inside the DFS. The answer should only increase **once** when a new island is first discovered.
    - Forgetting the boundary check before accessing the grid, which may cause an index out-of-range error.
    - Using a separate `visited` array is unnecessary in this solution since visited land cells are directly changed from `'1'` to `'0'`.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | August 18, 2026 |

## Python solution

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        res = 0

        def dfs(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == '0':
                return

            grid[i][j] = '0'
            dfs(i - 1, j)
            dfs(i, j - 1)
            dfs(i + 1, j)
            dfs(i, j + 1)

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    dfs(i, j)
                    res += 1

        return res
```

---

[Back to Day 44 Graph II](index.md) · [All Algorithm notes](../index.md)
