---
hide:
  - toc
---

# 121. Best Time to Buy and Sell Stock

*Edited by Charles*

**Easy · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Solution 1: DP

        **Step 1**: Define DP array states

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution 1: DP

    **Step 1**: Define DP array states

    dp[i][0]: the max cash at Iteration (i) if we are currently holding the stock;

    dp[i][1]: the max cash at Iteration (i) if we do not hold the stock;

    **Step 2**: Recursive formula

    dp[i][0] comes from two cases:

    1. dp[i - 1][0]: the stock was held before Iteration (i - 1) and nothing was done at Iteration (i)
    2. -prices[i]: the stock is bought at Iteration (i) and nothing was done before Iteration (i)

    dp[i][1] comes from two cases:

    1. dp[i - 1][1]: nothing was done at Iteration (i - 1), i.e. no stock at the moment (i)
    2. dp[i - 1][0] + prices[i]: stock was still held at Iteration (i - 1) and is sold at Iteration (i)

    **Step 3**: Initial States

    dp[0][0]: stock was held at the beginning, i.e. buying at Iteration (0), <u>-prices[0]</u>

    dp[0][1]: stock was not held at the beginning, i.e. nothing done at Iteration (0), <u>0</u>

    **Step 4**: The order of Iteration: only one iteration over prices sequentially.

    **Step 5**: Validate, given prices = [7, 1, 5, 3, 6 ,4]:

    ➡️ **Initial State**: dp[0][0] = -7 🆚 dp[0][1] = 0

    ➡️ **Iteration 0/Step 1**: dp[1][0] = max(dp[0][0], -prices[1]) = -1 🆚 dp[1][1] = max(dp[0][1], dp[0][0] + prices[1]) = 0

    ➡️ **Iteration 1/Step 2**: dp[2][0] = max(dp[1][0], -prices[2]) = -1 🆚 dp[2][1] = max(dp[1][1], dp[1][0] + prices[2]) = 4

    ➡️ **Iteration 2/Step 3**: dp[3][0] = max(dp[2][0], -prices[3]) = -1 🆚

    dp[3][1] = max(dp[2][1], dp[2][0] + prices[3]) = 4

    ➡️ **Iteration 3/Step 4**: dp[4][0] = max(dp[3][0], -prices[4]) = -1 🆚

    dp[4][1] = max(dp[3][1], dp[3][0] + prices[4]) = 5

    ➡️ **Iteration 4/Step 5**: dp[5][0] = max(dp[4][0], -prices[5]) = -1 🆚

    dp[5][1] = max(dp[4][1], dp[4][0] + prices[5]) = 5

    Code:

    ```jsx
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
    				    dp[i][0] = max(dp[i - 1][0], -prices[i])
    				    dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i])

    				# Return the final result: no stock held at the end
    				return dp[-1][1]
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | November 3, 2025 |
    | Feedback | Initialization of dp array (2-dimensional) in Python |

## Python solution

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        cost, profit = float('+inf'), 0
        for price in prices:
            cost = min(cost, price)
            profit = max(profit, price - cost)
        return profit
```

---

[Back to Day 35 Dynamic Programming VIII](index.md) · [All Algorithm notes](../index.md)
