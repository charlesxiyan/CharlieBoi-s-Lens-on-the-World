---
hide:
  - toc
---

# 153. Find Minimum in Rotated Sorted Array

*Edited by Charles*

**Medium · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/submissions/2136833837/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        You are given a sorted array of unique elements that has been rotated between `1` and `n` times.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    You are given a sorted array of unique elements that has been rotated between `1` and `n` times.

    For example, the original sorted array:

    ```text
    [0, 1, 2, 4, 5, 6, 7]
    ```

    may become:

    ```text
    [4, 5, 6, 7, 0, 1, 2]
    ```

    Return the minimum element of the array.

    The solution must run in:

    ```text
    O(log n)
    ```

    time.

    Because the original array is sorted and all elements are unique, after rotation the array consists of two sorted parts.

    For example:

    ```text
    [4, 5, 6, 7, 0, 1, 2]

     ↑        ↑  ↑     ↑
     sorted part  sorted part
    ```

    The minimum element is exactly the **rotation point**:

    ```text
    [4, 5, 6, 7, 0, 1, 2]
                 ↑
               minimum
    ```

    We can use Binary Search to locate this point.

    ## Key Idea

    At every step, compare:

    ```python
    nums[mid]
    ```

    with:

    ```python
    nums[right]
    ```

    There are two cases.

    ## Case 1 — `nums[mid] > nums[right]`

    If:

    ```python
    nums[mid] > nums[right]
    ```

    then the minimum must be strictly to the **right of `mid`**.

    Example:

    ```text
    [4, 5, 6, 7, 0, 1, 2]
     ↑        ↑        ↑
    left     mid      right

    nums[mid]   = 7
    nums[right] = 2

    7 > 2
    ```

    The drop from the larger sorted part to the smaller sorted part must occur somewhere after `mid`.

    Therefore:

    ```python
    left = mid + 1
    ```

    We can safely exclude `mid` because `nums[mid] > nums[right]`, so `mid` cannot be the minimum.

    ## Case 2 — `nums[mid] < nums[right]`

    If:

    ```python
    nums[mid] < nums[right]
    ```

    then the section from `mid` to `right` is sorted:

    ```text
    nums[mid] < ... < nums[right]
    ```

    Therefore, the minimum cannot be strictly to the right of `mid`.

    However, `mid` itself **may be the minimum**.

    Example:

    ```text
    [4, 5, 6, 7, 0, 1, 2]
                 ↑     ↑
                mid   right

    nums[mid]   = 0
    nums[right] = 2
    ```

    Here:

    ```text
    0 < 2
    ```

    and `mid` itself is exactly the minimum.

    Therefore:

    ```python
    right = mid
    ```

    Notice that we do **not** use:

    ```python
    right = mid - 1
    ```

    because doing so could accidentally remove the answer.

    The rightmost element gives us a convenient way to determine which sorted section `mid` belongs to.

    For a rotated array:

    ```text
    [4, 5, 6, 7, 0, 1, 2]
    ```

    we can think of the values as:

    ```text
    larger section        smaller section
    4  5  6  7            0  1  2
                              ↑
                           minimum
    ```

    Since `nums[right]` belongs to the smaller section:

    ```text
    nums[mid] > nums[right]
    ```

    means `mid` is still in the larger section, so we need to move right.

    ```text
    nums[mid] < nums[right]
    ```

    means `mid` is in the smaller section, so the minimum is at `mid` or somewhere to its left.

    We use:

    ```python
    while left < right:
    ```

    instead of:

    ```python
    while left <= right:
    ```

    because our search interval always contains the minimum.

    We continuously shrink:

    ```text
    [left, right]
    ```

    until:

    ```text
    left == right
    ```

    At that point, only one candidate remains.

    Therefore:

    ```python
    return nums[left]
    ```

    Consider:

    ```text
    nums = [4, 5, 6, 7, 0, 1, 2]
    ```

    Initially:

    ```text
    left = 0
    right = 6

    [4, 5, 6, 7, 0, 1, 2]
     ↑        ↑        ↑
     L       mid       R
    ```

    We have:

    ```text
    nums[mid]   = 7
    nums[right] = 2

    7 > 2
    ```

    Therefore:

    ```python
    left = mid + 1
    ```

    Now:

    ```text
    left = 4
    right = 6

    [4, 5, 6, 7, 0, 1, 2]
                 ↑  ↑  ↑
                 L mid R
    ```

    Now:

    ```text
    nums[mid]   = 1
    nums[right] = 2

    1 < 2
    ```

    Therefore:

    ```python
    right = mid
    ```

    Now:

    ```text
    left = 4
    right = 5

    [4, 5, 6, 7, 0, 1, 2]
                 ↑  ↑
                 L  R
    ```

    Calculate:

    ```text
    mid = 4
    ```

    So:

    ```text
    nums[mid]   = 0
    nums[right] = 1

    0 < 1
    ```

    Therefore:

    ```python
    right = mid
    ```

    Now:

    ```text
    left = 4
    right = 4
    ```

    So:

    ```text
    left == right
    ```

    and:

    ```python
    return nums[left]
    ```

    Result:

    ```text
    0
    ```

    For example:

    ```text
    [1, 2, 3, 4, 5]
    ```

    The same algorithm still works.

    Initially:

    ```text
    nums[mid]   = 3
    nums[right] = 5

    3 < 5
    ```

    So:

    ```python
    right = mid
    ```

    We continue searching toward the left until:

    ```text
    left == right == 0
    ```

    and return:

    ```text
    1
    ```

    No special case is required.

    ```text
    left = 0
    right = n - 1

            ↓

    while left < right

            ↓

    mid = (left + right) // 2

            ↓

    Compare nums[mid] with nums[right]

            ↓

    ┌─────────────────────────┐
    │                         │
    ↓                         ↓

    nums[mid] > nums[right]   nums[mid] < nums[right]

    minimum is strictly       minimum is at mid
    to the right              or to the left

    ↓                         ↓

    left = mid + 1            right = mid

              \               /
               \             /
                ↓           ↓

               left == right

                     ↓

             return nums[left]
    ```

    This is the most important detail of this problem.

    ### When `nums[mid] > nums[right]`

    `mid` definitely cannot be the minimum:

    ```text
    nums[mid] > nums[right]
    ```

    There is already a smaller value at `right`.

    Therefore:

    ```python
    left = mid + 1
    ```

    ### When `nums[mid] < nums[right]`

    `mid` could be the minimum.

    Therefore, we must keep `mid`:

    ```python
    right = mid
    ```

    not:

    ```python
    right = mid - 1
    ```

    ## Time Complexity

    Binary Search halves the search interval each time:

    ```text
    O(log n)
    ```

    ## Space Complexity

    Only a few variables are used:

    ```text
    O(1)
    ```

    For LeetCode 153, remember:

    ```text
    Compare nums[mid] with nums[right]
    ```

    Then:

    ```text
    nums[mid] > nums[right]
            ↓
    minimum is strictly on the right
            ↓
    left = mid + 1
    ```

    Otherwise:

    ```text
    nums[mid] < nums[right]
            ↓
    mid may be the minimum
            ↓
    right = mid
    ```

    Finally:

    ```text
    left == right
    ```

    and:

    ```python
    return nums[left]
    ```

    A compact way to remember the entire problem:

    ```text
    mid > right → go right → left = mid + 1

    mid < right → keep mid  → right = mid
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 9, 2026 |

## Python solution

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid

        return nums[left]
```

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
