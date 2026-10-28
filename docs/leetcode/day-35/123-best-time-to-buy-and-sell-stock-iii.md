---
hide:
  - toc
---

# 123. Best Time to Buy and Sell Stock III

*Edited by Charles*

**Hard · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

!!! tip "Core idea"

    Solution: DP

    **Step 1**: Define DP array states

## Python solution

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
      l = len(prices)
      if l == 0:
         return 0

      # Initiate states
      dp = [[0] * 5 for _ in range(l)]
      dp[0] = [0, -prices[0], 0, -prices[0], 0]

      # Iteration sequentially
      for i in range(1, l):
          dp[i][0] = dp[i - 1][0]
          dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i])
          dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + prices[i])
          dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i])
          dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i])

      return dp[-1][4]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution: DP

    **Step 1**: Define DP array states

    dp[i][0]: the max cash at Iteration (i) if we don’t do anything;

    dp[i][1]: the max cash at Iteration (i) as long as we hold the stock for the first time;

    dp[i][2]: the max cash at Iteration (i) as long as we do not hold the stock anymore for the first time;

    dp[i][3]: the max cash at Iteration (i) as long as we hold the stock for the second time;

    dp[i][4]: the max cash at Iteration (i) as long as we do not hold the stock anymore for the second time;

    **Step 2**: Recursive formula

    dp[i][0] comes from only one case:

    1. dp[i - 1][0]: nothing is done at Iteration (i - 1)

    dp[i][1] comes from two cases:

    1. dp[i - 1][1]: nothing is done at Iteration (i), i.e. still holding the stock at the moment (i) for the first time.
    2. dp[i - 1][0] - prices[i]: stock is bought at Iteration (i) and no transaction was done until Iteration (i)

    dp[i][2] comes from two cases:

    1. dp[i - 1][2]: nothing is done at Iteration (i), i.e. the position was kept closed at Iteration (i) since we do not hold the stock anymore for the first time (sold previously).
    2. dp[i - 1][1] + prices[i]: stock is sold at Iteration (i) for the first time.

    dp[i][3] comes from two cases:

    1. dp[i - 1][3]: nothing is done at Iteration (i), i.e. still holding the stock at the moment (i) for the second time.
    2. dp[i - 1][2] - prices[i]: stock is bought at Iteration (i) for the second time

    dp[i][4] comes from two cases:

    1. dp[i - 1][4]: nothing is done at Iteration (i), i.e. the position was kept closed at Iteration (i) since we do not hold the stock anymore for the second time (sold previously).
    2. dp[i - 1][3] + prices[i]: stock is sold at Iteration (i) for the second time.

    **Step 3**: Initial States

    dp[0][0]: 0 (do nothing)

    dp[0][1]: the stock is bought for the first time at the beginning, -prices[0]

    dp[0][2]: the stock is sold for the first time at the beginning, i.e. buy → sell at the same time unit, thus 0.

    dp[0][3]: the stock is bought for the second time immediately after the stock sold for the first time, -prices[0].

    dp[0][4]: the stock is sold for the second time immediately after the stock is bought for the second time, 0.

    **Step 4**: The order of Iteration: only one iteration over prices sequentially.

    **Step 5**: Validate, given prices = [7, 1, 5, 3, 6 ,4]:

    ➡️ **Initial State**:

    $dp[0][0] = 0 \\
    dp[0][1] = -7 \\
    dp[0][2] = 0 \\
    dp[0][3] = -7 \\
    dp[0][4] = 0 \\$

    ➡️ **Iteration 0/Step 1**:

    $dp[1][0] = dp[0][0] = 0 \\
    dp[1][1] = \max(dp[0][1], dp[0][0] - prices[1]) = -1 \\
    dp[1][2] = \max(dp[0][2], dp[0][1] + prices[1]) = 0 \\
    dp[1][3] = \max(dp[0][3], dp[0][2] - prices[1]) = -1 \\
    dp[1][4] = \max(dp[0][4], dp[0][3] + prices[1]) = 0 \\$

    ➡️ **Iteration 1/Step 2**:

    $dp[2][0] = dp[1][0] = 0 \\
    dp[2][1] = \max(dp[1][1], dp[1][0] - prices[2]) = -1 \\
    dp[2][2] = \max(dp[1][2], dp[1][1] + prices[2]) = 4 \\
    dp[2][3] = \max(dp[1][3], dp[1][2] - prices[2]) = -1 \\
    dp[2][4] = \max(dp[1][4], dp[1][3] + prices[2]) = 4 \\$

    ➡️ **Iteration 2/Step 3**:

    $dp[3][0] = dp[2][0] = 0 \\
    dp[3][1] = \max(dp[2][1], dp[2][0] - prices[3]) = -1 \\
    dp[3][2] = \max(dp[2][2], dp[2][1] + prices[3]) = 4 \\
    dp[3][3] = \max(dp[2][3], dp[2][2] - prices[3]) = 1 \\
    dp[3][4] = \max(dp[2][4], dp[2][3] + prices[3]) = 4 \\$

    ➡️ **Iteration 3/Step 4**:

    $dp[4][0] = dp[3][0] = 0 \\
    dp[4][1] = \max(dp[3][1], dp[3][0] - prices[4]) = -1 \\
    dp[4][2] = \max(dp[3][2], dp[3][1] + prices[4]) = 5 \\
    dp[4][3] = \max(dp[3][3], dp[3][2] - prices[4]) = 1 \\
    dp[4][4] = \max(dp[3][4], dp[3][3] + prices[4]) = 7 \\$

    ➡️ **Iteration 4/Step 5**:

    $dp[5][0] = dp[4][0] = 0 \\
    dp[5][1] = \max(dp[4][1], dp[4][0] - prices[5]) = -1 \\
    dp[5][2] = \max(dp[4][2], dp[4][1] + prices[5]) = 5 \\
    dp[5][3] = \max(dp[4][3], dp[4][2] - prices[5]) = 1 \\
    dp[5][4] = \max(dp[4][4], dp[4][3] + prices[5]) = 7 \\$

    In conclusion, we can validate

    1. dp[-1][1] infers the best initial buying moment no matter how many transactions in total, e.g. 1 is the valley of the price.
    2. dp[-1][2] infers the best initial selling moment no matter how many transactions in total, e.g. 6 is the peak of the price after the valley.
    3. dp[-1][3] infers the best second buying moment, and the moment does not have to follow the best initial selling moment.
    4. dp[-1][4] infers the best second selling moment, in order to keep the max cash at hand.

    Most importantly, dp[-1][4] incorporates the value of dp[-1][2], since dp[-1][4] at least is dp[-1][2] and then buy → sell immediately at the last moment. Intuitively, as long as there is a positive remain of cash after the second buying, dp[i][3], dp[i][4] will differ from dp[i][1], dp[i][2], i.e. considering a twice transaction; otherwise, one transaction is optimal (the first and second transaction always coincide, take [7, 1, 2, 4, 10, 6] as an example).

    Code:

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | November 3, 2025 |
    | Feedback | Basically an extension of 121 and 122, but need more attention to understanding the chain effect of 1st buy, 1st sell, 2nd buy, and 2nd sell. |

---

[Back to Day 35 Dynamic Programming VIII](index.md) · [All LeetCode notes](../index.md)
