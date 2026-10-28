---
hide:
  - toc
---

# 33. Search in Rotated Sorted Array

*Edited by Charles*

**Medium · Python · Array**

[Original problem](https://leetcode.com/problems/search-in-rotated-sorted-array/)

!!! tip "Core idea"

    Use **Binary Search**.

    Although the rotated array is not completely sorted, after splitting the current interval at `mid`, **at least one half must be sorted**.

## Python solution

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid

            # Left half is sorted
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1

            # Right half is sorted
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Binary Search**.

    Although the rotated array is not completely sorted, after splitting the current interval at `mid`, **at least one half must be sorted**.

    For example:

    ```text
    nums = [4, 5, 6, 7, 0, 1, 2]
    ```

    If:

    ```text
    left = 0
    mid = 3
    right = 6
    ```

    then:

    ```text
    [4, 5, 6, 7] | [0, 1, 2]
     ↑        ↑       ↑     ↑
    left     mid     mid+1 right
    ```

    The left half is sorted.

    Therefore, in every iteration:

    1. Determine which half is sorted.
    2. Check whether `target` lies inside that sorted half.
    3. Decide which half to continue searching.

    ## Step 1: Check Whether Target Is Found

    As in normal binary search:

    ```python
    if nums[mid] == target:
        return mid
    ```

    ## Step 2: Determine Which Half Is Sorted

    ### Left Half Is Sorted

    If:

    ```python
    nums[left] <= nums[mid]
    ```

    then the interval:

    ```text
    [left, mid]
    ```

    is sorted.

    For example:

    ```text
    [4, 5, 6, 7, 0, 1, 2]
     ↑        ↑
    left     mid
    ```

    Since:

    ```text
    4 <= 7
    ```

    the left half:

    ```text
    [4, 5, 6, 7]
    ```

    is sorted.

    ## Why Use `<=` Instead of `<`?

    We must use:

    ```python
    nums[left] <= nums[mid]
    ```

    instead of:

    ```python
    nums[left] < nums[mid]
    ```

    because `left` can be equal to `mid`.

    For example:

    ```text
    nums = [3, 1]
    target = 1

    left = 0
    right = 1
    mid = 0
    ```

    Now:

    ```text
    left == mid
    ```

    and:

    ```text
    nums[left] == nums[mid] == 3
    ```

    The interval `[left, mid]` contains only one element, so it is still sorted.

    Therefore:

    ```python
    nums[left] <= nums[mid]
    ```

    correctly identifies the left half as sorted.

    ## Step 3: Check Whether Target Is Inside the Sorted Half

    ### Case 1: Left Half Is Sorted

    We know:

    ```python
    nums[left] <= nums[mid]
    ```

    Now check whether `target` is inside this sorted interval:

    ```python
    if nums[left] <= target < nums[mid]:
    ```

    If true:

    ```python
    right = mid - 1
    ```

    Otherwise:

    ```python
    left = mid + 1
    ```

    So:

    ```text
    Left half is sorted

    nums[left] <= target < nums[mid]
            ↓
    target is in the left half
            ↓
    right = mid - 1

    Otherwise
            ↓
    target must be in the right half
            ↓
    left = mid + 1
    ```

    ### Case 2: Right Half Is Sorted

    If the left half is not sorted, then the right half must be sorted.

    Check whether `target` lies inside:

    ```python
    if nums[mid] < target <= nums[right]:
    ```

    If true:

    ```python
    left = mid + 1
    ```

    Otherwise:

    ```python
    right = mid - 1
    ```

    So:

    ```text
    Right half is sorted

    nums[mid] < target <= nums[right]
            ↓
    target is in the right half
            ↓
    left = mid + 1

    Otherwise
            ↓
    target must be in the left half
            ↓
    right = mid - 1
    ```

    ## Example

    ```text
    nums = [4, 5, 6, 7, 0, 1, 2]
    target = 0
    ```

    ### Round 1

    ```text
    index:  0  1  2  3  4  5  6
    nums:   4  5  6  7  0  1  2
            ↑        ↑        ↑
           left     mid      right
    ```

    We have:

    ```text
    nums[left] = 4
    nums[mid] = 7
    ```

    Since:

    ```text
    4 <= 7
    ```

    the left half is sorted:

    ```text
    [4, 5, 6, 7]
    ```

    Check whether `target = 0` is inside:

    ```text
    4 <= 0 < 7
    ```

    False.

    Therefore, search the right half:

    ```python
    left = mid + 1
    ```

    Now:

    ```text
    left = 4
    right = 6
    ```

    ### Round 2

    ```text
    index:  0  1  2  3  4  5  6
    nums:   4  5  6  7  0  1  2
                        ↑  ↑  ↑
                       left mid right
    ```

    Now:

    ```text
    nums[left] = 0
    nums[mid] = 1
    ```

    Since:

    ```text
    0 <= 1
    ```

    the left half is sorted.

    Check:

    ```text
    0 <= target < 1
    ```

    For `target = 0`:

    ```text
    0 <= 0 < 1
    ```

    True.

    Therefore:

    ```python
    right = mid - 1
    ```

    Now:

    ```text
    left = 4
    right = 4
    ```

    ### Round 3

    ```text
    nums[4] = 0
    ```

    Therefore:

    ```python
    return 4
    ```

    ## Binary Search Logic

    The entire logic can be summarized as:

    ```text
                     nums[mid] == target
                             ↓
                        return mid

                   Which half is sorted?
                            / \
                           /   \
                        Left   Right
                         ↓       ↓
                  Is target   Is target
                  inside it?  inside it?
                     / \         / \
                   Yes  No     Yes  No
                    ↓    ↓      ↓    ↓
                  Left Right  Right Left
    ```

    The key idea is:

    > **Find the sorted half first, then determine whether the target lies inside that sorted half.**
    >

    ## Complexity

    **Time Complexity:**

    ```text
    O(log n)
    ```

    Each iteration eliminates half of the current search interval.

    **Space Complexity:**

    ```text
    O(1)
    ```

    ## Key Points

    ### At least one half is sorted

    After choosing `mid`, either:

    ```text
    [left, mid]
    ```

    or:

    ```text
    [mid, right]
    ```

    must be sorted.

    ### Determine the sorted half first

    ```python
    if nums[left] <= nums[mid]:
        # left half is sorted
    else:
        # right half is sorted
    ```

    ### Then check whether target is inside it

    Left sorted:

    ```python
    nums[left] <= target < nums[mid]
    ```

    Right sorted:

    ```python
    nums[mid] < target <= nums[right]
    ```

    ### Use `<=`

    ```python
    nums[left] <= nums[mid]
    ```

    because when:

    ```text
    left == mid
    ```

    the left interval contains one element and is still sorted.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 8, 2026 |

---

[Back to Bonus Array](index.md) · [All LeetCode notes](../index.md)
