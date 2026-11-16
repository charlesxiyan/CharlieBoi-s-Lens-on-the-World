---
hide:
  - toc
---

# 120. Triangle

*Edited by Charles*

**Medium · Python · Dynamic Programming · Bottom-up DP**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/triangle/description/).

    Find the smallest sum along a path from the top of a triangle to its bottom. From column `j`, the next row allows column `j` or `j + 1`.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Work upward instead of enumerating paths. Before processing row `i`, `dp[j]` stores the minimum cost from the cell immediately below at column `j` to the bottom. The two possible children already have optimal costs.

    ## Algorithm Steps

    1. Copy the last row into `dp`: a bottom cell has no further choices.
    2. Visit rows from the second-last row to the top.
    3. Set `dp[j] = triangle[i][j] + min(dp[j], dp[j + 1])`.
    4. Return `dp[0]`, the minimum cost from the top.

    ## Hand Simulation

    For `[[2], [3,4], [6,5,7], [4,1,8,3]]`:

    | Row processed | Active DP values |
    | --- | --- |
    | Bottom row | `[4,1,8,3]` |
    | `[6,5,7]` | `[7,6,10]` |
    | `[3,4]` | `[9,10]` |
    | `[2]` | `[11]` |

    The path `2 → 3 → 5 → 1` has sum `11`.

    ## Important Implementation Details

    Update `j` from left to right. Both `dp[j]` and `dp[j + 1]` then still represent the row below when they are read. Right-to-left updates would overwrite a needed child. The slice `[:]` prevents mutation of the triangle. Negative entries are also handled: the recurrence compares full remaining path costs.

    ## Time and Space Complexity

    For $R$ rows, time is $O(R^2)$ and auxiliary space is $O(R)$.

    ## Key Takeaways

    When a state depends only on the next row, bottom-up DP can reuse one array.

## Python solution

```python
class Solution:
    def minimumTotal(self, triangle: list[list[int]]) -> int:
        l = len(triangle)

        dp = triangle[-1][:]
        for i in range(l - 2, -1, -1):
            for j in range(i + 1):
                dp[j] = triangle[i][j] + min(dp[j], dp[j + 1])

        return dp[0]
```

---

[Back to Bonus Dynamic Programming](index.md) · [All Algorithm notes](../index.md)
