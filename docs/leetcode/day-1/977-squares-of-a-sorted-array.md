---
hide:
  - toc
---

# 977. Squares of a Sorted Array

*Edited by Charles*

**Easy · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/squares-of-a-sorted-array/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    **Don't Overcomplicate**: Keep the logic simple and avoid unnecessary complexity. **Edge Case**: Consider the scenario where the array contains only one element.

## Python solution

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [0 for _ in range(n)]

        l, r, k = 0, n - 1, n - 1
        while l <= r:
            if nums[l] + nums[r] < 0:
                res[k] = nums[l] ** 2
                l += 1
            else:
                res[k] = nums[r] ** 2
                r -= 1
            k -= 1

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ### **LeetCode 977. Squares of a Sorted Array**

    1. **Don't Overcomplicate**: Keep the logic simple and avoid unnecessary complexity.
    2. **Edge Case**: Consider the scenario where the array contains only one element.
    3. **Output Visualization**: Define a result array `res`. The output is essentially the original array with its elements reordered; therefore, you can initialize `res` as an array of zeros with the same length as the input.
    4. **Left and Right Pointers**: Use two pointers, `l` (left) and `r` (right), to rewrite the indices `k` of the new array starting from the end (back-to-front).
    5. **Core Logic**: Since the input array may contain negative numbers, the largest squared values must originate from either the far left or the far right of the original array. In other words, **the largest square will be either the square of the leftmost element or the square of the rightmost element**.
    6. **Universal Applicability**: This logic inherently covers cases where the original array is entirely non-negative or entirely non-positive.
    7. **Determining Magnitude**: The sum `nums[l] + nums[r]` can directly determine whether the absolute value on the left or the right is larger, as the entire array is already sorted. A negative sum implies the absolute value on the left is greater, and vice versa.
    8. **Execution**: Based on the logic above, move the left and right pointers inward to select numbers, square them, and place them into the new array from back to front.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 4 |
    | Mastery | ❚❚❚❚ |
    | The Latest Attempt | December 31, 2025 |
    | Feedback | 完全没想到用左右双指针，需要多练 |

---

[Back to Day 1 Array I](index.md) · [All Algorithm notes](../index.md)
