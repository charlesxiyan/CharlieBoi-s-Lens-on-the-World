---
hide:
  - toc
---

# 279. Perfect Squares

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/perfect-squares/)

!!! tip "Core idea"

    - This is a **Complete Knapsack** problem.
    - Each perfect square (`1, 4, 9, ...`) can be used **unlimited times**.
    - Goal: find the minimum number of perfect squares whose sum equals `n`.

## Python solution

```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n + 1)
        dp[0] = 0

        for i in range(1, int(n ** 0.5) + 1):
            for j in range(i * i, n + 1):
                dp[j] = min(dp[j - i * i] + 1, dp[j])

        return dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    - This is a **Complete Knapsack** problem.
    - Each perfect square (`1, 4, 9, ...`) can be used **unlimited times**.
    - Goal: find the minimum number of perfect squares whose sum equals `n`.

    ## DP Definition

    dp[j] = minimum number of perfect squares needed to form sum `j`.

    Initialize:
    dp = [inf] * (n + 1)
    dp[0] = 0

    ## Why `inf`?

    - `inf` means **currently unreachable**.
    - Initially, only `0` is reachable.
    - `inf` works naturally with `min()` and represents an impossible state.

    ## Iteration

    ### Outer loop

    for i in range(1, int(sqrt(n)) + 1):

    - `i` represents the base number.
    - The current item is `i * i`.
    - The upper bound is `sqrt(n)` because any square larger than `n` can never be used.

    ### Inner loop

    for j in range(i * i, n + 1):

    - Start from `i * i` because sums smaller than the current square cannot use it.
    - Iterate **forward** to allow using the same square multiple times.
    - This is the characteristic of **Complete Knapsack**.

    ## Transition

    dp[j] = min(dp[j], dp[j - i * i] + 1)

    Meaning:

    - Keep the current answer.
    - Or use one more current perfect square.

    ## Complexity

    Time: O(n√n)

    Space: O(n)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 9, 2026 |

---

[Back to Day 33 Dynamic Programming VI](index.md) · [All LeetCode notes](../index.md)
