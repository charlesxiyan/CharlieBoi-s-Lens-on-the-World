---
hide:
  - toc
---

# 62. Unique Paths

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/unique-paths/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recall [70. Climbing Stairs](../day-28/70-climbing-stairs.md) for the core idea. Emphasis: the initialization. Consider the boundaries, say grid[0][j] and grid[i][0], there is only one way to reach the places along these boundaries.

## Python solution

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        grid = [[1] * n for _ in range(m)]

        for i in range(1, m):
            for j in range(1, n):
                grid[i][j] = grid[i - 1][j] + grid[i][j - 1]

        return grid[-1][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [70. Climbing Stairs](../day-28/70-climbing-stairs.md) for the core idea.
    2. Emphasis: the initialization. Consider the boundaries, say grid[0][j] and grid[i][0], there is only one way to reach the places along these boundaries.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 2, 2026 |

---

[Back to Day 29 Dynamic Programming II](index.md) · [All Algorithm notes](../index.md)
