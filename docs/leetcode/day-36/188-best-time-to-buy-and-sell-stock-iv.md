---
hide:
  - toc
---

# 188. Best Time to Buy and Sell Stock IV

*Edited by Charles*

**Hard · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

!!! tip "Core idea"

    Solution: DP

    An extension or an abstract version of Q123.

## Python solution

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
      l = len(prices)
      if l == 0:
        return 0

      # Initiate
      dp = [[0] * (2 * k + 1) for _ in range(l)]
      dp[0][1:] = [-prices[0], 0] * k

      for i in range(1, l):
        for j in range(1, 2 * k + 1):
          if j % 2 == 1:
            dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1] - prices[i])
          else:
            dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1] + prices[i])


      return dp[-1][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution: DP

    An extension or an abstract version of Q123.

    **One transaction = 2 more states (state of holding the stock for the k-th time, and the state of not holding the stock anymore for the k-th time)**

    **Key:**

    dp[i][0]: nothing is done

    dp[i][m]: max cash as long as the stock is held for the m-th time, m = 1, 3, 5, …, 2k - 1 is odd.

    dp[i][m + 1]: max cash as long as the stock is held for the m-th time, m m + 1 = 2, 4, 6, …, 2k is even.

    Thus, we need a (len(prices), 2k + 1) array.

    Recursive formula for each odd number m:

    $dp[i][m] = \max(dp[i - 1][m], dp[i - 1][m - 1] - prices[i]) \\
    dp[i][m + 1] = \max(dp[i - 1][m + 1], dp[i - 1][m] + prices[i])$

    Code:

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | November 4, 2025 |
    | Feedback | An extension of 123, but be careful of the recursive formula when it comes to k transactions. |

---

[Back to Day 36 Dynamic Programming IX](index.md) · [All Algorithm notes](../index.md)
