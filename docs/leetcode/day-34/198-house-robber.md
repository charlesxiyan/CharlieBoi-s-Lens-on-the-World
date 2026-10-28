---
hide:
  - toc
---

# 198. House Robber

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/house-robber/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    - Dynamic Programming.
    - For each house, there are only **two states**:
    - Rob this house.
    - Do not rob this house.
    - The answer is the maximum profit after considering all houses.

## Python solution

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        l = len(nums)

        if l == 0:
            return 0

        dp = [[0] * 2 for _ in range(l)]
        dp[0][0] = nums[0]

        for i in range(1, l):
		        # 抢劫第i个房屋，最大金额为前一个房屋不抢劫的最大金额加上当前房屋的金额
            dp[i][0] = dp[i - 1][1] + nums[i]
            # 不抢劫第i个房屋，最大金额为前一个房屋抢劫和不抢劫的最大值
            dp[i][1] = max(dp[i - 1][0], dp[i - 1][1])

        return max(dp[-1])
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    - Dynamic Programming.
    - For each house, there are only **two states**:
        - Rob this house.
        - Do not rob this house.
    - The answer is the maximum profit after considering all houses.

    ## DP Definition

    dp[i][0] = maximum money if we **rob** house `i`.

    dp[i][1] = maximum money if we **do not rob** house `i`.

    ## Initialization

    If there is only one house:

    dp[0][0] = nums[0]
    dp[0][1] = 0

    ## Transition

    ### Rob house i

    To rob house `i`, house `i-1` must **not** be robbed.

    dp[i][0] = dp[i-1][1] + nums[i]

    ### Do not rob house i

    If we skip house `i`, house `i-1` may or may not have been robbed.

    dp[i][1] = max(dp[i-1][0], dp[i-1][1])

    ## Why these transitions?

    - Robbing two adjacent houses is not allowed.
    - Therefore:
        - **Rob current → Previous must NOT be robbed.**
        - **Skip current → Previous can be either robbed or skipped.**

    ## Iteration

    Process houses from left to right.

    For each house:

    1. Compute the value if we rob it.
    2. Compute the value if we skip it.

    ## Complexity

    Time: O(n)

    Space: O(n)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 4, 2026 |

---

[Back to Day 34 Dynamic Programming VII](index.md) · [All Algorithm notes](../index.md)
