---
hide:
  - toc
---

# 213. House Robber II

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/house-robber-ii/description/)

!!! tip "Core idea"

    - Houses are arranged in a **circle**.
    - Therefore, the **first and last houses cannot both be robbed**.
    - Convert the circular problem into two linear House Robber (198) problems.

    Case 1:
    Rob from house `0` to `n-2`
    (Exclude the last house.)

## Python solution

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        l = len(nums)
        if l < 3:
            return max(nums)

        max1 = self.robBasic(nums[:-1])
        max2 = self.robBasic(nums[1:])

        return max(max1, max2)


    def robBasic(self, nums):
        l = len(nums)
        dp = [[0] * 2 for _ in range(l)]
        dp[0][0] = nums[0]

        for i in range(1, l):
            # 抢劫第i个房屋，最大金额为前一个房屋不抢劫的最大金额加上当前房屋的金额
            # max(dp[i - 1][1] + nums[i], dp[i - 1][1]) = dp[i][0] = dp[i - 1][1] + nums[i]
            dp[i][0] = dp[i - 1][1] + nums[i]
            # 不抢劫第i个房屋，最大金额为前一个房屋抢劫和不抢劫的最大值
            dp[i][1] = max(dp[i - 1][0], dp[i - 1][1])

        return max(dp[-1])
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    - Houses are arranged in a **circle**.
    - Therefore, the **first and last houses cannot both be robbed**.
    - Convert the circular problem into two linear House Robber (198) problems.

    Case 1:
    Rob from house `0` to `n-2`
    (Exclude the last house.)

    Case 2:
    Rob from house `1` to `n-1`
    (Exclude the first house.)

    The answer is:

    max(case1, case2)

    ## Why does this work?

    Since the first and last houses are adjacent, every valid solution must satisfy one of the following:

    - The first house is excluded.
    - The last house is excluded.

    These two cases cover all possible valid solutions.

    ## Base Case

    If there are fewer than 3 houses:

    return max(nums)

    ## DP Definition (Same as LeetCode 198)

    dp[i][0] = maximum money if house `i` is robbed.

    dp[i][1] = maximum money if house `i` is not robbed.

    ## Transition

    Rob current house:

    dp[i][0] = dp[i-1][1] + nums[i]

    Skip current house:

    dp[i][1] = max(dp[i-1][0], dp[i-1][1])

    ## Complexity

    Time: O(n)

    Space: O(n)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 34 Dynamic Programming VII](index.md) · [All LeetCode notes](../index.md)
