---
hide:
  - toc
---

# 64. Minimum Path Sum

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/minimum-path-sum/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Refer to [62. Unique Paths](../day-29/62-unique-paths.md) for the same idea.

## Python solution

```python
class Solution:
    def minPathSum(self, grid: list[list[int]]) -> int:
        m, n = len(grid), len(grid[0])

        dp = [[0] * n for _ in range(m)]
        dp[0][0] = grid[0][0]

        for i in range(1, m):
            dp[i][0] = dp[i - 1][0] + grid[i][0]

        for j in range(1, n):
            dp[0][j] = dp[0][j - 1] + grid[0][j]

        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]

        return dp[-1][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Refer to [62. Unique Paths](../day-29/62-unique-paths.md) for the same idea.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 17, 2026 |

---

[Back to Bonus Dynamic Programming](index.md) · [All Algorithm notes](../index.md)
