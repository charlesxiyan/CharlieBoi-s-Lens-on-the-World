---
hide:
  - toc
---

# 309. Best Time to Buy and Sell Stock with Cooldown

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Solution: DP

    **Step 1**: Define DP array states

## Python solution

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        l = len(prices)

        # Initiate
        dp = [[0] * 4 for _ in range(l)]
        dp[0][0] = -prices[0]

        for i in range(1, l):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i], dp[i - 1][3] - prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][3])
            dp[i][2] = dp[i - 1][0] + prices[i]
            dp[i][3] = dp[i - 1][2]

        return max(dp[-1][1:])
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution: DP

    **Step 1**: Define DP array states

    dp[i][0]: the max cash at Iteration (i) if we hold the stock;

    dp[i][1]: the max cash at Iteration (i) after the cooldown day if we do not hold the stock;

    dp[i][2]: the max cash at Iteration (i) if we sell the stock on this day;

    dp[i][3]: the max cash at Iteration (i) on the cooldown day;

    **Step 2**: Recursive formula

    dp[i][0] comes from three cases:

    1. dp[i - 1][1] - prices[i]: we passed the cooldown but did not hold the stock until today
    2. dp[i - 1][3] - prices[i]: previous day was the cooldown day and we buy the stock today
    3. dp[i - 1][0]: we already held the stock

    dp[i][1] comes from two cases:

    1. dp[i - 1][1]: we already passed the cooldown day and even yesterday was not the cooldown day.
    2. dp[i - 1][3]: yesterday was the cooldown day.

    dp[i][2] comes from one cases:

    1. dp[i - 1][0] + prices[i]: we hold the stock until today

    dp[i][3] comes from one cases:

    1. dp[i - 1][2]: the cooldown day is exactly one day after the sell day.

    **Step 3**: Initial States

    dp[0][0]: -prices[i]

    dp[0][1]: 0 (can be inferred from the formula of dp[0][0]; this is an illegal state, since no stock was held yet.)

    dp[0][2]: 0 (can be inferred from the formula of dp[0][3]; this is an illegal state, since no stock was held yet.).

    dp[0][3]: 0 (can be inferred from the formula of dp[0][0]; this is an illegal state, since no stock was held yet.)

    **Step 4**: The order of Iteration: only one iteration over prices sequentially.

    **Step 5**: Validate, given prices = [1, 2, 3, 0, 2]:

    ➡️ **Initial State**:

    $dp[0][0] = -1 \\
    dp[0][1] = 0 \\
    dp[0][2] = 0 \\
    dp[0][3] = 0 \\$

    ➡️ **Iteration 0/Step 1**:

    $dp[1][0] = \max(dp[0][0], dp[0][1] - prices[1], dp[0][3] - prices[1]) = -1 \\
    dp[1][1] = \max(dp[0][1], dp[0][3]) = 0 \\
    dp[1][2] = dp[0][0] + prices[1] = 1 \\
    dp[1][3] = dp[0][2] = 0 \\$

    ➡️ **Iteration 1/Step 2**:

    $dp[2][0] = \max(dp[1][0], dp[1][1] - prices[2], dp[1][3] - prices[2]) = -1 \\
    dp[2][1] = \max(dp[1][1], dp[1][3]) = 0 \\
    dp[2][2] = dp[1][0] + prices[2] = 2 \\
    dp[2][3] = dp[1][2] = 1 \\$

    ➡️ **Iteration 2/Step 3**:

    $dp[3][0] = \max(dp[2][0], dp[2][1] - prices[3], dp[2][3] - prices[3]) = 1 \\
    dp[3][1] = \max(dp[2][1], dp[2][3]) = 1 \\
    dp[3][2] = dp[2][0] + prices[3] = -1 \\
    dp[3][3] = dp[2][2] = 2 \\$

    ➡️ **Iteration 3/Step 4**:

    $dp[4][0] = \max(dp[3][0], dp[3][1] - prices[4], dp[3][3] - prices[4]) = 1 \\
    dp[4][1] = \max(dp[3][1], dp[3][3]) = 2 \\
    dp[4][2] = dp[3][0] + prices[4] = 3 \\
    dp[4][3] = dp[3][2] = -1 \\$

    return max(dp[4][1], dp[4][2], dp[4][3])

    Code:

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | November 6, 2025 |
    | Feedback | An extension of 122 but need to be very clear of all states |

---

[Back to Day 36 Dynamic Programming IX](index.md) · [All Algorithm notes](../index.md)
