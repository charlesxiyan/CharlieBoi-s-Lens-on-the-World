---
hide:
  - toc
---

# 343. Integer Break

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/integer-break/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Exceptions: 2 → 1 + 1 → product 1; 3 → 1 + 2 → product 2. DP Steps:

    dp[i]: the maximum value of products by adding components.

## Python solution

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n == 2:
            return 1
        if n == 3:
            return 2
        dp = [0] * (n + 1)
        dp[2], dp[3] = 1, 2

        for i in range(4, n + 1):
            for j in range(1, i // 2 + 1):
                dp[i] = max(dp[i], j * (i - j), j * dp[i - j])

        return dp[n]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Exceptions: 2 → 1 + 1 → product 1; 3 → 1 + 2 → product 2.
    2. DP Steps:
        1. **Step 1**: Define DP array states

            dp[i]: the maximum value of products by adding components.

            Note that the length of dp should be n + 1.

        2. **Step 2**: Recursive formula (n ≥ 4, since n = 2, 3, the maximum products are deterministic)

            dp[i] only comes from the below cases:

            1. j * (i - j) (no more decomposition)
            2. j * dp[i - j] (potential larger product from further decomposition)
            3. dp[i] (update itself, since there are two loops, i and j)
        3. **Step 3**: Initial States

            dp[0] = 0

            dp[1] = 0

            dp[2] = 1

            dp[3] = 2

        4. **Step 4**: The order of Iteration:
            1. iteration from number 4 to n.
            2. iteration from j to [i // 2], but attention: in python, in order to iterate [i // 2], we need to use

                ```python
                ### Omit 0, since it won't be help with updates.
                range(1, i // 2 + 1)
                ```

        5. **Step 5**: Validation is omitted.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 2, 2026 |

---

[Back to Day 29 Dynamic Programming II](index.md) · [All Algorithm notes](../index.md)
