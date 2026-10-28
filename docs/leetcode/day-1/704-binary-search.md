---
hide:
  - toc
---

# 704. Binary Search

*Edited by Charles*

**Easy · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/binary-search/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Maintain a closed search interval. Compare the middle value with the target and discard the half that cannot contain it.

## Python solution

```python
# Simplify
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l = len(nums)

        left, right, middle = 0, l - 1, (l - 1) // 2

        while left <= right:
            if nums[left] == target:
                return left
            elif nums[right] == target:
                return right
            elif nums[middle] == target:
                return middle

            if nums[middle] < target:
                left = middle + 1
            else:
                right = middle - 1

            middle = (left + right) // 2

        return -1
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. Boundary check!
        1. the length of the array is 1, and then we just need to check if the only element is the target.
        2. check if the last element in the array is the target.
    2. Define three pointers, left: l; middle: m; right: r. For a given subarray, l is the index of the first element in the subarray, r is the index of the last one, and m is the floor division of l + r by 2.
    3. The loop condition is l < m < r, i.e. l + 1 < r. That is, the subarray needs to at least keep the form [nums[l], nums[m], nums[r]]; otherwise, break the loop.
    4. Within the loop,
        1. check if nums[m] is the target. If yes, return m; if no, divide the array.
        2. after partitioning the array into 2 subarrays, we have two disjoint subarrays, [nums[l], nums[m - 1]], and [nums[m], nums[r]].
        3. update l, m and r. For example, if we need to keep the second subarray, then r does not need any changes. Update l to the current m and recalculate m by (l + r) // 2.
        4. Check boundaries if the target is there.

    ```python
    # Redundant Version
    class Solution:
        def search(self, nums: List[int], target: int) -> int:
            l = len(nums)
            if l == 1:
                if nums[0] == target:
                    return 0
                else:
                    return -1

            if l == 2:
                if nums[0] == target:
                    return 0
                elif nums[1] == target:
                    return 1
                else:
                    return -1

            left, right, middle = 0, l - 1, l // 2

            while left < right - 1:
                if nums[left] == target:
                    return left
                elif nums[right] == target:
                    return right
                elif nums[middle] == target:
                    return middle

                if nums[middle] < target:
                    left = middle
                else:
                    right = middle

                middle = (left + right + 1) // 2

            return -1
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 4 |
    | Mastery | ❚❚❚❚ |
    | The Latest Attempt | December 31, 2025 |
    | Feedback | 标准二分法搜索 |

---

[Back to Day 1 Array I](index.md) · [All Algorithm notes](../index.md)
