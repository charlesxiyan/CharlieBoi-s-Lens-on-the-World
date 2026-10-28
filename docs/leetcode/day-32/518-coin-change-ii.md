---
hide:
  - toc
---

# 518. Coin Change II

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/coin-change-ii/)

!!! tip "Core idea"

    A complete knapsack problem. The core idea is the same as [494. Target Sum](../day-31/494-target-sum.md).

## Python solution

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        n = len(coins)

        dp = [0] * (amount + 1)
        dp[0] = 1
        for i in range(n):
            for j in range(coins[i], amount + 1):
                dp[j] += dp[j - coins[i]]

        return dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. A complete knapsack problem.
    2. The core idea is the same as [494. Target Sum](../day-31/494-target-sum.md).
    3. However, since duplicates coins are allowed, we iterate from left to right.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 6, 2026 |

---

[Back to Day 32 Dynamic Programming V](index.md) · [All Algorithm notes](../index.md)
