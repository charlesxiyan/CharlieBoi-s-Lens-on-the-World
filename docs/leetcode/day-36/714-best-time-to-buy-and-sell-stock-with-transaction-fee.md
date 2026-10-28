---
hide:
  - toc
---

# 714. Best Time to Buy and Sell Stock with Transaction Fee

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Solution: DP

    Exactly the same as 122 but need to be careful of the transaction fee deduction; see the recursive formula below.

## Python solution

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        l = len(prices)

        # Initiate
        dp = [[0] * 2 for _ in range(l)]
        dp[0] = [-prices[0], 0]

        for i in range(1, l):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee)

        return dp[-1][1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution: DP

    Exactly the same as 122 but need to be careful of the transaction fee deduction; see the recursive formula below.

    The transaction only pops up in

    $dp[i][1] = \max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee)$

    Code:

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | November 6, 2025 |
    | Feedback | A little tweak based on 122. Easy to handle. |

---

[Back to Day 36 Dynamic Programming IX](index.md) · [All Algorithm notes](../index.md)
