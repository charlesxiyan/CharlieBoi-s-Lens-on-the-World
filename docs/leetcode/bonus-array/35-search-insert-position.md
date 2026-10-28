---
hide:
  - toc
---

# 35. Search Insert Position

*Edited by Charles*

**Easy · Python · Array**

[Original problem](https://leetcode.com/problems/search-insert-position/description/)

!!! tip "Core idea"

    Use **Binary Search**.

    If `target` exists in the sorted array, return its index.

## Python solution

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        mid = (left + right) // 2

        while left <= right:
            if nums[mid] == target:
                return mid

            if nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1

            mid = (left + right) // 2

        return left
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Binary Search**.

    If `target` exists in the sorted array, return its index.

    If `target` does not exist, return the position where it should be inserted so that the array remains sorted.

    ## Binary Search

    Initialize:

    ```python
    left, right = 0, len(nums) - 1
    ```

    The current search interval is:

    ```text
    [left, right]
    ```

    Calculate:

    ```python
    mid = (left + right) // 2
    ```

    There are three cases.

    ### Case 1: `nums[mid] == target`

    We found the target:

    ```python
    return mid
    ```

    ### Case 2: `nums[mid] > target`

    Since the array is sorted, `target` must be on the left side of `mid`.

    ```python
    right = mid - 1
    ```

    ### Case 3: `nums[mid] < target`

    `target` must be on the right side of `mid`.

    ```python
    left = mid + 1
    ```

    ## Why Return `left`?

    If `target` does not exist, binary search eventually ends when:

    ```text
    left > right
    ```

    At this point:

    ```text
    ... elements < target | elements > target ...
                          ↑
                         left
    ```

    `left` is exactly the position where `target` should be inserted.

    Another way to understand it:

    ```text
    right | left
            ↑
       insertion position
    ```

    During binary search:

    ```python
    if nums[mid] < target:
        left = mid + 1
    ```

    `left` keeps moving past elements that are definitely smaller than `target`.

    Therefore, when the search finishes, `left` is the first position where `target` can be inserted.

    ## Example

    ```text
    nums = [1, 3, 5, 6]
    target = 2
    ```

    Initially:

    ```text
    index    0   1   2   3
    nums     1   3   5   6
             ↑           ↑
            left       right
    ```

    ### Round 1

    ```text
    left = 0
    right = 3
    mid = 1

    nums[mid] = 3
    ```

    Since:

    ```text
    3 > 2
    ```

    move `right`:

    ```python
    right = mid - 1 = 0
    ```

    ### Round 2

    ```text
    left = 0
    right = 0
    mid = 0

    nums[mid] = 1
    ```

    Since:

    ```text
    1 < 2
    ```

    move `left`:

    ```python
    left = mid + 1 = 1
    ```

    Now:

    ```text
    right = 0
    left = 1
    ```

    Therefore:

    ```text
    1 | 3
      ↑
     left
    ```

    `2` should be inserted between `1` and `3`, so:

    ```python
    return left  # 1
    ```

    ## Boundary Cases

    If `target` is smaller than every element:

    ```text
    nums = [1, 3, 5, 6]
    target = 0

    left = 0
    ```

    Insert at the beginning.

    If `target` is larger than every element:

    ```text
    nums = [1, 3, 5, 6]
    target = 7

    left = 4
    ```

    Insert at the end.

    Thus, `return left` naturally handles all insertion positions.

    ## Complexity

    **Time Complexity:**

    ```text
    O(log n)
    ```

    The search interval is reduced by half after each iteration.

    **Space Complexity:**

    ```text
    O(1)
    ```

    Only a few variables are used.

    ## Key Points

    Binary search interval:

    ```text
    [left, right]
    ```

    Update rules:

    ```text
    nums[mid] > target
    → target is on the left
    → right = mid - 1

    nums[mid] < target
    → target is on the right
    → left = mid + 1
    ```

    If the target is not found:

    ```text
    right | left
            ↑
       insertion position
    ```

    Therefore:

    ```python
    return left
    ```

    **Key intuition:** `left` keeps moving past elements that are definitely smaller than `target`. When binary search finishes, `left` is exactly where `target` should be inserted.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 8, 2026 |

---

[Back to Bonus Array](index.md) · [All LeetCode notes](../index.md)
