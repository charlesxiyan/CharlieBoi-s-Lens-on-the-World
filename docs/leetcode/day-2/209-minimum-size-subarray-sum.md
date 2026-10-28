---
hide:
  - toc
---

# 209. Minimum Size Subarray Sum

*Edited by Charles*

**Medium · Python · Array**

[Original problem](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

!!! tip "Core idea"

    A typical sliding window problem!

    The object is the length of **a subarray that contains contiguous elements with sum greater than a given target number**.

## Python solution

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        n = len(nums)
        start, end = 0, 0
        s = 0
        length = float('inf')

        while end < n:
            s += nums[end]
            end += 1

            while s >= target and start <= end:
                length = min(end - start, length)
                s -= nums[start]
                start += 1

        return 0 if length == float('inf') else length
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    A typical sliding window problem!

    The object is the length of **a subarray that contains contiguous elements with sum greater than a given target number**.

    Routine:

    Q1. When to expand the window?

    Answer: when the sum of all elements in the subarray is less than the given target number.

    Q2. When to shrink the window?

    Answer: when the sum of all elements in the subarray is greater than the target number and the beginning index of the subarray is no greater than the ending index.

    Q3. When to update the object?

    Answer: when the window is shrunk, the length of the subarray can be updated.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 4 |
    | Mastery | ❚❚❚❚ |
    | The Latest Attempt | January 2, 2026 |

---

[Back to Day 2 Array II](index.md) · [All LeetCode notes](../index.md)
