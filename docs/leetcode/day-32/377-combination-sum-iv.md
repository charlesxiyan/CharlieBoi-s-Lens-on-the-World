---
hide:
  - toc
---

# 377. Combination Sum IV

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/combination-sum-iv/description/)

!!! tip "Core idea"

    Simialr to [518. Coin Change II](518-coin-change-ii.md). However, it should be noted that the iteration order in this solution is fixed, i.e.

## Python solution

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        n = len(nums)
        dp = [0] * (target + 1)
        dp[0] = 1

        for i in range(1, target + 1):
            for j in range(n):
                if i - nums[j] >= 0:
                    dp[i] += dp[i - nums[j]]

        return dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Simialr to [518. Coin Change II](518-coin-change-ii.md).
    2. However, it should be noted that the iteration order in this solution is fixed, i.e.
        1. iterate over the knapsack volume/size.
        2. iterate over the items to be stuffed into the knapsack.
        3. The reason why we need to do this is: consider an iteration of items [1, 2, 5] first, and then bag size, when i = 1, for different bag sizes, say 3, we should have combinations [1, 2] (pick up 1 first and then iterate the next item 2, jumping from left to right) and [2, 1] (pick up 2 first and jump back to 1). However, if we fix the item first, i.e. the first item [1], in the for loop of bag size, say 3, the jump is always driven from left to right and miss [2, 1].

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 6, 2026 |

---

[Back to Day 32 Dynamic Programming V](index.md) · [All Algorithm notes](../index.md)
