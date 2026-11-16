---
hide:
  - toc
---

# 162. Find Peak Element

*Edited by Charles*

**Medium · Python · Binary Search · Slope**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-peak-element/description/).

    Return the index of any element strictly larger than its neighbors. Adjacent elements are unequal, and the values outside the array are treated as negative infinity. The required running time is logarithmic.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Compare neighboring values at the middle. An uphill edge guarantees a peak on its right: the sequence must eventually turn downward or reach the right boundary. A downhill edge guarantees a peak at the middle or to its left. The whole array need not be sorted.

    ## Algorithm Steps

    1. Initialize a closed interval `[left, right]`.
    2. While it contains more than one index, compare `nums[mid]` with `nums[mid + 1]`.
    3. If the edge rises, keep `[mid + 1, right]`; otherwise keep `[left, mid]`.
    4. Return the sole remaining index.

    ## Hand Simulation

    For `[1,2,3,1]`: `[0,3]` gives `mid = 1`; `2 < 3`, so `left = 2`. Then `[2,3]` gives `mid = 2`; `3 > 1`, so `right = 2`. Return index `2`.

    ## Important Implementation Details

    The maintained interval has an uphill entry from its left exterior and a downhill exit to its right exterior, with negative infinity at original boundaries. Thus it always contains a peak. Because `left < right`, `mid < right`, so `mid + 1` is valid. Keep `mid` in the downhill branch: it may itself be the peak. A one-element array immediately returns `0`.

    ## Time and Space Complexity

    Time is $O(\log N)$ and auxiliary space is $O(1)$.

    ## Key Takeaways

    Binary search needs a justified way to discard half the candidates, not necessarily a globally sorted array.

## Python solution

```python
class Solution:
    def findPeakElement(self, nums: list[int]) -> int:
        left, right = 0, len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if nums[mid] < nums[mid + 1]:
                left = mid + 1
            else:
                right = mid
        return left
```

---

[Back to Bonus Binary Search](index.md) · [All Algorithm notes](../index.md)
