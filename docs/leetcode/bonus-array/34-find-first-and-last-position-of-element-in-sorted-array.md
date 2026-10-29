---
hide:
  - toc
---

# 34. Find First and Last Position of Element in Sorted Array

*Edited by Charles*

**Medium · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Use **Binary Search twice**:

        Find the **leftmost** occurrence of `target`. Find the **rightmost** occurrence of `target`.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Binary Search twice**:

    1. Find the **leftmost** occurrence of `target`.
    2. Find the **rightmost** occurrence of `target`.

    The key difference from normal binary search is:

    > When `target` is found, do not immediately return. Continue searching for the boundary.
    >

    ## Find the Left Boundary

    Use standard binary search:

    ```python
    left, right = 0, len(nums) - 1
    ```

    If:

    ```python
    nums[mid] == target
    ```

    record the current position:

    ```python
    res = mid
    ```

    But this may not be the first occurrence.

    For example:

    ```text
    nums = [5, 7, 7, 8, 8, 10]
                        ↑
                       mid
    ```

    Even if we find `8` at index `4`, there may still be another `8` on the left.

    Therefore:

    ```python
    res = mid
    right = mid - 1
    ```

    This means:

    ```text
    Found target
        ↓
    Record current index
        ↓
    Continue searching left
    ```

    So for the left boundary:

    ```text
    nums[mid] == target
    → save mid
    → move right to mid - 1
    ```

    ## Find the Right Boundary

    The logic is symmetric.

    If:

    ```python
    nums[mid] == target
    ```

    record the position:

    ```python
    res = mid
    ```

    Then continue searching to the right:

    ```python
    left = mid + 1
    ```

    because there may be another occurrence of `target` later in the array.

    So for the right boundary:

    ```text
    nums[mid] == target
    → save mid
    → move left to mid + 1
    ```

    ## Example

    ```text
    nums = [5, 7, 7, 8, 8, 10]
    target = 8
    ```

    The target range is:

    ```text
    index:  0  1  2  3  4   5
    nums:   5  7  7  8  8  10
                      ↑  ↑
                    left right
    ```

    ### Left Boundary

    Suppose binary search finds an `8`.

    Instead of returning immediately:

    ```python
    res = mid
    right = mid - 1
    ```

    Continue searching the left half until no earlier `8` exists.

    Result:

    ```text
    left boundary = 3
    ```

    ### Right Boundary

    When an `8` is found:

    ```python
    res = mid
    left = mid + 1
    ```

    Continue searching the right half until no later `8` exists.

    Result:

    ```text
    right boundary = 4
    ```

    Therefore:

    ```python
    [3, 4]
    ```

    ## If Target Does Not Exist

    Initialize:

    ```python
    res = -1
    ```

    If `target` is never found, `res` remains `-1`.

    For example:

    ```text
    nums = [5, 7, 7, 8, 8, 10]
    target = 6
    ```

    Neither binary search finds `6`, so:

    ```python
    [-1, -1]
    ```

    ## Key Difference from Normal Binary Search

    Normal binary search:

    ```text
    nums[mid] == target
    → return mid
    ```

    LeetCode 34:

    ```text
    Find left boundary:

    nums[mid] == target
    → record mid
    → continue left

    Find right boundary:

    nums[mid] == target
    → record mid
    → continue right
    ```

    The most important idea is:

    ```text
    Finding target ≠ finding the boundary
    ```

    We must continue binary search after finding `target`.

    ## Complexity

    We perform binary search twice.

    **Time Complexity:**

    ```text
    O(log n)
    ```

    because:

    ```text
    O(log n) + O(log n) = O(log n)
    ```

    **Space Complexity:**

    ```text
    O(1)
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 8, 2026 |

## Python solution

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:

        def findLeft():
            left, right = 0, len(nums) - 1
            res = -1

            while left <= right:
                mid = (left + right) // 2

                if nums[mid] == target:
                    res = mid
                    right = mid - 1
                elif nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1

            return res

        def findRight():
            left, right = 0, len(nums) - 1
            res = -1

            while left <= right:
                mid = (left + right) // 2

                if nums[mid] == target:
                    res = mid
                    left = mid + 1
                elif nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1

            return res

        return [findLeft(), findRight()]
```

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
