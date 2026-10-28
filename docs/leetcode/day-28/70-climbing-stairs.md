---
hide:
  - toc
---

# 70. Climbing Stairs

*Edited by Charles*

**Easy · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/climbing-stairs/)

!!! tip "Core idea"

    DP Steps:

    **Step 1**: Define DP array states

## Python solution

```python
class Solution:
    def climbStairs(self, n: int, m: int) -> int:
        dp = [0] * (n + 1)
        dp[0] = 1

        for i in range(1, n + 1):
		        for j in range(i, m + 1):
				        if i >= j:
						        dp[i] += dp[i - j]

        return dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    DP Steps:

    **Step 1**: Define DP array states

    dp[i]: the number of ways to reach level n.

    Note that the length of dp should be n + 1.

    **Step 2**: Recursive formula (n ≥ 3)

    dp[i] only comes from the summation of two elements:

    1. dp[i - 1]: since all ways to reach i - 1 just need one more step to reach i
    2. dp[i - 2]: since all ways to reach i - 2 just need two more steps directly to reach i

    **Step 3**: Initial States

    dp[1] = 1

    dp[2] = 2

    **Step 4**: The order of Iteration: only one iteration over index from 3 to n sequentially.

    **Step 5**: Validation is omitted.

    ```python
    class Solution:
        def climbStairs(self, n: int) -> int:
            if n <= 2:
                return n

            dp = [0] * (n + 1)
            dp[1] = 1
            dp[2] = 2

            for i in range(3, n + 1):
                dp[i] = dp[i - 1] + dp[i - 2]

            return dp[-1]
    ```

    Knapsack Solution:

    1. If 1, 2, …., m steps are allowed for stair climbing, this question now turns to a knapsack problem.
    2. The sources of dp[i] are dp[i - 1], dp[i - 2], ……, dp[i - m].
        1. The recursive function thus is dp[i] += dp[i - j] for j in range(1, m + 1)
    3. dp[0] is the basis of other components in the list. The initial value of dp[0] should be 1.
    4. Since [1, 2] and [2, 1] are different paths, then in the iteration, the knapsack size should be iterated in the outer loop.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | January 31, 2026 |

---

[Back to Day 28 Dynamic Programming I](index.md) · [All LeetCode notes](../index.md)
