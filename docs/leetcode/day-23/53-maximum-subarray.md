---
hide:
  - toc
---

# 53. Maximum Subarray

*Edited by Charles*

**Medium · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/maximum-subarray/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        At each position, either extend the previous subarray or start a new one. Track the largest ending-here sum seen so far.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution (DP):

    1. Define DP array.
        1. dp[i] is the maximum sum so far (inclusive of nums[i])
    2. Recursive formula:
        1. dp[i] comes from two cases:
            1. dp[i - 1] + nums[i]
            2. nums[i]
    3. Initial States:
        1. dp[0] only has one source, nums[0] itself.
    4. Iteration over nums from 1 to len(nums) - 1.
    5. Validation: Omitted!

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 26, 2026 |

## Python solution

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        l = len(nums)
        if l == 0:
            return 0

        dp = [0] * l
        dp[0] = nums[0]
        result = dp[0]

        for i in range(1, l):
            dp[i] = max(dp[i - 1] + nums[i], nums[i])
            if dp[i] > result:
                result = dp[i]

        return result
```

---

[Back to Day 23 Greedy Algorithm I](index.md) · [All Algorithm notes](../index.md)
