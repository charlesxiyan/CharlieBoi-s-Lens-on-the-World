---
hide:
  - toc
---

# 509. Fibonacci Number

*Edited by Charles*

**Easy · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/fibonacci-number/description/)

!!! tip "Core idea"

    DP Steps:

    **Step 1**: Define DP array states

## Python solution

```python
class Solution:
    def fib(self, n: int) -> int:
        if n == 0 or n == 1:
            return n

        dp = [0] * (n + 1)
        dp[1] = 1

        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]

        return dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    DP Steps:

    **Step 1**: Define DP array states

    dp[i]: the value of Fibonacci number at index i.

    Note that the length of dp should be n + 1.

    **Step 2**: Recursive formula (n ≥ 2)

    dp[i] only comes from the summation of two elements:

    1. dp[i - 1]
    2. dp[i - 2]

    **Step 3**: Initial States

    dp[0] = 0

    dp[1] = 1

    **Step 4**: The order of Iteration: only one iteration over index from 2 to n sequentially.

    **Step 5**: Validation is omitted.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 31, 2026 |

---

[Back to Day 28 Dynamic Programming I](index.md) · [All Algorithm notes](../index.md)
