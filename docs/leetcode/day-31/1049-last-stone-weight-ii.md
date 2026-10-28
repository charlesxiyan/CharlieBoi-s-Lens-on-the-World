---
hide:
  - toc
---

# 1049. Last Stone Weight II

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/last-stone-weight-ii/description/)

!!! tip "Core idea"

    Recall [416. Partition Equal Subset Sum](../day-30/416-partition-equal-subset-sum.md) for the core idea. The solution in this question partition the list into two subsets with equal values or closest difference.

## Python solution

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        n = len(stones)
        total = sum(stones)

        target = total // 2

        dp = [0] * (target + 1)

        for i in range(n):
            for j in range(target, stones[i] - 1, -1):
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])

        return total - 2 * dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recall [416. Partition Equal Subset Sum](../day-30/416-partition-equal-subset-sum.md) for the core idea. The solution in this question partition the list into two subsets with equal values or closest difference.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 5, 2026 |

---

[Back to Day 31 Dynamic Programming IV](index.md) · [All LeetCode notes](../index.md)
