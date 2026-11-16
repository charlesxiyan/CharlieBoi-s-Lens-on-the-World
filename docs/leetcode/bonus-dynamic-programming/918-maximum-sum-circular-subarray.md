---
hide:
  - toc
---

# 918. Maximum Sum Circular Subarray

*Edited by Charles*

**Medium · Python · Dynamic Programming · Kadane’s algorithm**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/maximum-sum-circular-subarray/description/).

    Find the maximum sum of a nonempty contiguous subarray in a circular array. A subarray may wrap around the end, but cannot use an element twice.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    There are two shapes: an ordinary subarray, or a suffix joined to a prefix. A wrapping subarray is the whole array with a middle subarray removed. Its best candidate sum is therefore `sum(nums) - minSum`.

    ## Algorithm Steps

    1. Compute the best ordinary subarray with maximum-sum Kadane DP.
    2. If that sum is negative, return it immediately.
    3. Compute the minimum ordinary subarray using the analogous minimum recurrence.
    4. Compare the ordinary maximum with the total minus the minimum.

    ## Hand Simulation

    For `[5,-3,5]`, maximum-ending-here values are `[5,2,7]`; minimum-ending-here values are `[5,-3,2]`. The total is `7`, so excluding `[-3]` gives `7 - (-3) = 10`, using the last and first `5`.

    For `[-3,-2,-5]`, the ordinary maximum is `-2`. The early return prevents incorrectly choosing the empty complement with sum `0`.

    ## Important Implementation Details

    In each helper, `dp[i]` must end at index `i`: either start at `nums[i]` or extend the previous state. Taking `max(dp)` or `min(dp)` then considers every endpoint. Although the minimum helper initializes with negative infinity, every entry is overwritten before it is read, so that initialization does not affect the result. If `maxSum == 0`, a nonempty zero-sum answer exists; the early return only needs `< 0`.

    ## Time and Space Complexity

    For $N$ elements, time is $O(N)$ and auxiliary space is $O(N)$ for the supplied DP arrays.

    ## Key Takeaways

    Circular optimization often becomes an ordinary optimum compared with the complement of the opposite optimum. Check that the complement is not empty.

## Python solution

```python
class Solution:
    def maxSubarraySumCircular(self, nums: list[int]) -> int:
        maxSum = self.maxSubarray(nums)
        if maxSum < 0:
            return maxSum
        minSum = self.minSubarray(nums)
        return max(maxSum, sum(nums) - minSum)
    def maxSubarray(self, nums):
        l = len(nums)
        dp = [-float('inf')] * l
        dp[0] = nums[0]
        for i in range(1, l):
            dp[i] = max(nums[i], dp[i - 1] + nums[i])
        return max(dp)
    def minSubarray(self, nums):
        l = len(nums)
        dp = [-float('inf')] * l
        dp[0] = nums[0]
        for i in range(1, l):
            dp[i] = min(nums[i], dp[i - 1] + nums[i])
        return min(dp)
```

---

[Back to Bonus Dynamic Programming](index.md) · [All Algorithm notes](../index.md)
