---
hide:
  - toc
---

# 474. Ones and Zeroes

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/ones-and-zeroes/description/)

!!! tip "Core idea"

    dp[i][j]: the size of the largest subset with at most number i of 0s and number j of 1s. dp[i][j] = max(dp[i][j], dp[i - numZero][j - numOne] + 1), where numZero and numOne are nubmers of 0s and 1s in the last iterated string (for s in strs).

## Python solution

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for s in strs:
            zeroNum = s.count('0')
            oneNum = len(s) - zeroNum

            for i in range(m, zeroNum - 1, -1):
                for j in range(n, oneNum - 1, -1):
                    dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1)

        return dp[-1][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. dp[i][j]: the size of the largest subset with at most number i of 0s and number j of 1s.
    2. dp[i][j] = max(dp[i][j], dp[i - numZero][j - numOne] + 1), where numZero and numOne are nubmers of 0s and 1s in the last iterated string (for s in strs).

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 5, 2026 |

---

[Back to Day 31 Dynamic Programming IV](index.md) · [All Algorithm notes](../index.md)
