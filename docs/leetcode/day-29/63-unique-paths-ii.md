---
hide:
  - toc
---

# 63. Unique Paths II

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/unique-paths-ii/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recall [62. Unique Paths](62-unique-paths.md) for the core idea. Then we need to deal with paths that connect the obstacles.

    ```python
    ### Boundary Example
    obstacleGrid[0][1] = [0, 0, 1, 0, 0]
    ### Initialization
    grid[0][0] = 1
    ### Boundary Values
    grid[0][1] = grid[0][0] = 1

## Python solution

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if obstacleGrid[0][0] == 1 or obstacleGrid[-1][-1] == 1:
            return 0

        m, n = len(obstacleGrid), len(obstacleGrid[0])
        grid = [[0] * n for _ in range(m)]
        grid[0][0] = 1

        for i in range(1, m):
            if obstacleGrid[i][0] == 0:
                grid[i][0] = grid[i - 1][0]

        for j in range(1, n):
            if obstacleGrid[0][j] == 0:
                grid[0][j] = grid[0][j - 1]

        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 1:
                    continue

                grid[i][j] = grid[i - 1][j] + grid[i][j - 1]

        return grid[-1][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [62. Unique Paths](62-unique-paths.md) for the core idea.
    2. Then we need to deal with paths that connect the obstacles.
        1. consider exceptions: start grid[0][0] and grid[-1][-1] end are obstacles. No feasible path.
        2. grid is initialized as a 2-dimensional array of zeros.
        3. for regular cases, initialize the start grid[0][0] as 1.
        4. iterate boundaries, and the grid components on the boundaries are identical to their previous components if the current position does not have obstacles; once an obstacle pops up, the value passing will stop and all the incoming boundary components will be zeroes. Take below as an example.

            ```python
            ### Boundary Example
            obstacleGrid[0][1] = [0, 0, 1, 0, 0]
            ### Initialization
            grid[0][0] = 1
            ### Boundary Values
            grid[0][1] = grid[0][0] = 1

            ### Since obstacleGrid[0][2] == 1
            ### No value passing from the previous component, i.e. grid[0][i] == grid[0][i - 1]
            grid[0][2] # keeps the original value 0

            ### For i >= 3, restore grid[0][i] == grid[0][i - 1] since there is no more obstacles.
            ### However, only 0 can be passed since there is an obstacle at [0][2].
            ```

        5. for any position off the boundaries and has an obstacle, simply skip it since no path should be connect to this position.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 2, 2026 |

---

[Back to Day 29 Dynamic Programming II](index.md) · [All Algorithm notes](../index.md)
