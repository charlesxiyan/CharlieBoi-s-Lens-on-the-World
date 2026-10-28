---
hide:
  - toc
---

# 122. Best Time to Buy and Sell Stock II

*Edited by Charles*

**Medium · Python · Greedy**

[Original problem](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

!!! tip "Core idea"

    Solution 1: DP

    **Step 1**: Define DP array states

## Python solution

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Initialize dp array and the states
        l = len(prices)
        if l == 0:
          return 0
        ##############################################
        max = 0

        for i in range(1, l):
            daily_profit = prices[i] - prices[i - 1]
            if daily_profit > 0:
                max += daily_profit

        return max

# Elegant

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profits = [prices[i] - prices[i - 1] for i in range(1, len(prices))]
        res = 0

        for profit in profits:
            res += profit if profit > 0 else 0

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution 1: DP

    **Step 1**: Define DP array states

    dp[i][0]: the max cash at Iteration (i) if we are currently holding the stock;

    dp[i][1]: the max cash at Iteration (i) if we do not hold the stock;

    **Step 2**: Recursive formula

    dp[i][0] comes from two cases:

    1. dp[i - 1][0]: the stock was held before Iteration (i - 1)
    2. dp[i - 1][1] - prices[i]: the stock is bought at Iteration (i), and no share was held before Iteration (i).

    dp[i][1] comes from two cases:

    1. dp[i - 1][1]: nothing was done at Iteration (i - 1), i.e. no stock at the moment (i)
    2. dp[i - 1][0] + prices[i]: stock was still held at Iteration (i - 1) and is sold at Iteration (i)

    **Step 3**: Initial States

    dp[0][0]: stock was held at the beginning, i.e. buying at Iteration (0), <u>-prices[0]</u>

    dp[0][1]: stock was not held at the beginning, i.e. nothing done at Iteration (0), <u>0</u>

    **Step 4**: The order of Iteration: only one iteration over prices sequentially.

    **Step 5**: Validate, given prices = [7, 1, 5, 3, 6 ,4]:

    ➡️ **Initial State**: dp[0][0] = -7 🆚 dp[0][1] = 0

    ➡️ **Iteration 0/Step 1**: dp[1][0] = max(dp[0][0], dp[0][1] - prices[1]) = -1 🆚 dp[1][1] = max(dp[0][1], dp[0][0] + prices[1]) = 0

    ➡️ **Iteration 1/Step 2**: dp[2][0] = max(dp[1][0], dp[1][1] - prices[2]) = -1 🆚 dp[2][1] = max(dp[1][1], dp[1][0] + prices[2]) = 4

    ➡️ **Iteration 2/Step 3**: dp[3][0] = max(dp[2][0], dp[2][1] - prices[3]) = 1 🆚

    dp[3][1] = max(dp[2][1], dp[2][0] + prices[3]) = 4

    ➡️ **Iteration 3/Step 4**: dp[4][0] = max(dp[3][0], dp[3][1] - prices[4]) = 1 🆚

    dp[4][1] = max(dp[3][1], dp[3][0] + prices[4]) = 7

    ➡️ **Iteration 4/Step 5**: dp[5][0] = max(dp[4][0], dp[4][1] - prices[5]) = 3 🆚

    dp[5][1] = max(dp[4][1], dp[4][0] + prices[5]) = 7

    Code:

    ```python
    class Solution:
        def maxProfit(self, prices: List[int]) -> int:
            # Initialize dp array and the states
            l = len(prices)
            if l == 0:
              return 0
            ##############################################
            dp = [[0] * 2 for _ in range(l)]
            ##############################################
            dp[0][0], dp[0][1] = -prices[0], 0
            # Iteration
            for i in range(1, l):
                dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i])
                dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i])

            # Return the final result: no stock held at the end
            return dp[-1][1]
    ```

    Solution 2: Caveat

    Since we can buy and sell for multiple times and they can happen on the same day but the only constraint is only one share is allowed to hold, i.e. buy → sell or sell → buy, instead of buy → buy.

    Code:

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 3 |
    | Mastery | ❚❚❚ |
    | The Latest Attempt | January 27, 2026 |

---

[Back to Day 24 Greedy Algorithm II](index.md) · [All Algorithm notes](../index.md)
