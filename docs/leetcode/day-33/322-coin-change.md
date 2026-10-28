---
hide:
  - toc
---

# 322. Coin Change

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/coin-change/description/)

!!! tip "Core idea"

    - This is a **Complete Knapsack** problem.
    - Each coin can be used **unlimited times**.
    - Goal: minimize the number of coins needed to make `amount`.

## Python solution

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        l = len(coins)

        dp = [float('inf')] * (amount + 1)
        dp[0] = 0

        for i in range(l):
            for j in range(coins[i], amount + 1):
                if dp[j - coins[i]] != float('inf'):
                    dp[j] = min(dp[j], dp[j - coins[i]] + 1)

        return -1 if dp[-1] == float('inf') else dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    - This is a **Complete Knapsack** problem.
    - Each coin can be used **unlimited times**.
    - Goal: minimize the number of coins needed to make `amount`.

    ## DP Definition

    dp[j] = minimum number of coins needed to make amount `j`.

    Initialize:
    dp = [inf] * (amount + 1)
    dp[0] = 0

    ## Why `inf`?

    - `inf` represents an **unreachable state**.
    - Initially, only amount `0` is reachable.
    - `inf` works naturally with `min()`:
        - `min(inf, x) = x`
        - `inf + 1 = inf`
    - This keeps the transition clean and avoids extra edge-case handling.
    - If `dp[amount]` is still `inf` at the end, the amount cannot be formed.

    ## Transition

    For each coin:
    for j from coin to amount:
    if dp[j - coin] != inf:
    dp[j] = min(dp[j], dp[j - coin] + 1)

    Meaning:

    - Don't extend from an unreachable state.
    - Either keep the current answer or use one more current coin.

    ## Why iterate `j` forward?

    Forward iteration allows using the **same coin multiple times**.
    This is the characteristic of **Complete Knapsack**.

    Backward iteration would become **0/1 Knapsack**, where each item can only be used once.

    ## Complexity

    Time: O(n × amount)

    Space: O(amount)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 9, 2026 |

---

[Back to Day 33 Dynamic Programming VI](index.md) · [All Algorithm notes](../index.md)
