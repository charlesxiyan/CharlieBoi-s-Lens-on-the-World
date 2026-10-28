---
hide:
  - toc
---

# 1005. Maximize Sum of Array after K Negations

*Edited by Charles*

**Easy · Python · Greedy**

[Original problem](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)

!!! tip "Core idea"

    Sort the numbers and flip negative values first. If an odd number of flips remains, flip the value with the smallest absolute magnitude.

## Python solution

```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        nums.sort(key = lambda x: abs(x), reverse = True)

        for i in range(len(nums)):
            if nums[i] < 0 and k > 0:
                nums[i] *= -1
                k -= 1

        if k % 2:
            nums[-1] *= -1

        return sum(nums)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. This solution is quite fabulous!
    2. See steps below:
        1. reorder nums by their absolute values in descending order.
        2. iterate the list, find negative numbers and change their signs; update k by k - 1 per change.
            1. if the remaining k is even, there is nothing to do since changing the sign of the smallest value even times is equivalent to doing nothing.
            2. if the remianing k is odd, just change the sign of the smallest value once to keep the summation least influenced by the negative numbers.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 27, 2026 |

---

[Back to Day 24 Greedy Algorithm II](index.md) · [All Algorithm notes](../index.md)
