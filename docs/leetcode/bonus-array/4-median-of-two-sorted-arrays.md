---
hide:
  - toc
---

# 4. Median of Two Sorted Arrays

*Edited by Charles*

**Hard · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Binary-search a partition of the shorter array. Find a split where every left-side value is no larger than every right-side value, then read the middle value(s).

## Python solution

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        if len(nums1) > len(nums2):
            nums1, nums2 = nums2, nums1

        m, n = len(nums1), len(nums2)

        left, right = 0, m

        while left <= right:
            i = (left + right) // 2
            j = (m + n + 1) // 2 - i

            leftA = nums1[i - 1] if i > 0 else -float('inf')
            rightA = nums1[i] if i < m else float('inf')

            leftB = nums2[j - 1] if j > 0 else -float('inf')
            rightB = nums2[j] if j < n else float('inf')

            if leftA <= rightB and leftB <= rightA:
                if (m + n) % 2:
                    return max(leftA, leftB)
                else:
                    return (
                        max(leftA, leftB)
                        + min(rightA, rightB)
                    ) / 2

            elif leftA > rightB:
                right = i - 1

            else:
                left = i + 1
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Binary Search + Partition**.

    Instead of actually merging the two sorted arrays, divide both arrays into a **left part** and a **right part** such that:

    1. The left part contains half of all elements.
    2. Every element in the left part is smaller than or equal to every element in the right part.

    Then the median can be determined directly from the elements around the two partitions.

    To achieve `O(log(min(m, n)))`, always perform binary search on the **shorter array**.

    ## Always Binary Search the Shorter Array

    Let:

    ```text
    m = len(nums1)
    n = len(nums2)
    ```

    Make sure:

    ```text
    m <= n
    ```

    Therefore:

    ```python
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    ```

    We only binary search the partition position in `nums1`.

    The possible partition positions are:

    ```text
    0, 1, 2, ..., m
    ```

    Therefore:

    ```python
    left, right = 0, m
    ```

    ## Partition the Two Arrays

    Suppose `i` is the partition position in `nums1`:

    ```text
    nums1:

    [ ... leftA ] | [ rightA ... ]
                    ↑
                    i
    ```

    `i` also represents the **number of elements from `nums1` placed in the left half**.

    Choose:

    ```python
    i = (left + right) // 2
    ```

    The total number of elements required in the left half is:

    ```python
    (m + n + 1) // 2
    ```

    Therefore, if `nums1` contributes `i` elements, `nums2` must contribute:

    ```python
    j = (m + n + 1) // 2 - i
    ```

    So:

    ```text
    i + j = (m + n + 1) // 2
    ```

    The `+1` allows the left half to contain one extra element when the total number of elements is odd.

    ## Four Boundary Values

    After partitioning:

    ```text
    nums1:  ... leftA | rightA ...
    nums2:  ... leftB | rightB ...
    ```

    Define:

    ```python
    leftA = nums1[i - 1]
    rightA = nums1[i]

    leftB = nums2[j - 1]
    rightB = nums2[j]
    ```

    However, the partition may occur at the beginning or end of an array.

    For example:

    ```text
    | 1 2 3
    ```

    There is no left element, so treat it as:

    ```text
    -inf
    ```

    Similarly:

    ```text
    1 2 3 |
    ```

    has no right element, so treat it as:

    ```text
    +inf
    ```

    Therefore:

    ```python
    leftA = nums1[i - 1] if i > 0 else -float('inf')
    rightA = nums1[i] if i < m else float('inf')

    leftB = nums2[j - 1] if j > 0 else -float('inf')
    rightB = nums2[j] if j < n else float('inf')
    ```

    ## Correct Partition Condition

    Because both arrays are already sorted, we already know:

    ```text
    leftA <= rightA
    leftB <= rightB
    ```

    Therefore, we only need to check the cross-array relationships:

    ```text
    leftA <= rightB
    leftB <= rightA
    ```

    If both conditions hold:

    ```text
    nums1:  ... leftA | rightA ...
    nums2:  ... leftB | rightB ...

                  correct partition
    ```

    then every element in the combined left half is less than or equal to every element in the combined right half.

    ## Binary Search Direction

    ### Case 1: `leftA > rightB`

    ```text
    nums1:  ... leftA | rightA
    nums2:  ... leftB | rightB

    leftA > rightB
    ```

    `nums1` contributes **too many elements** to the left half.

    Therefore, `i` is too large and should move left:

    ```python
    right = i - 1
    ```

    ### Case 2: `leftB > rightA`

    This means `nums1` contributes **too few elements** to the left half.

    Therefore, `i` is too small and should move right:

    ```python
    left = i + 1
    ```

    A useful way to remember:

    ```text
    leftA > rightB
    → i is too large
    → move left

    leftB > rightA
    → i is too small
    → move right
    ```

    ## Calculate the Median

    Once the correct partition is found:

    ```text
    nums1: ... leftA | rightA ...
    nums2: ... leftB | rightB ...
    ```

    ### Odd Total Length

    Because the left half contains one extra element:

    ```text
    median = largest element in the left half
    ```

    Therefore:

    ```python
    max(leftA, leftB)
    ```

    ### Even Total Length

    The two middle elements are:

    ```text
    largest element in the left half
    = max(leftA, leftB)

    smallest element in the right half
    = min(rightA, rightB)
    ```

    Therefore:

    ```python
    (max(leftA, leftB) + min(rightA, rightB)) / 2
    ```

    ## Example

    ```text
    nums1 = [1, 3]
    nums2 = [2]
    ```

    Since `nums1` must be the shorter array, swap them:

    ```text
    nums1 = [2]
    nums2 = [1, 3]

    m = 1
    n = 2
    ```

    The left half needs:

    ```text
    (m + n + 1) // 2
    = 4 // 2
    = 2
    ```

    elements.

    First partition:

    ```text
    i = 0
    j = 2
    ```

    So:

    ```text
    nums1:       | 2
    nums2:  1 3  |
    ```

    We have:

    ```text
    leftA  = -∞
    rightA = 2

    leftB  = 3
    rightB = +∞
    ```

    Since:

    ```text
    leftB > rightA
    3 > 2
    ```

    `nums1` contributes too few elements to the left side.

    Move `i` right.

    Next:

    ```text
    i = 1
    j = 1
    ```

    Partition:

    ```text
    nums1:  2 |
    nums2:  1 | 3
    ```

    Now:

    ```text
    leftA  = 2
    rightA = +∞

    leftB  = 1
    rightB = 3
    ```

    Both conditions hold:

    ```text
    2 <= 3
    1 <= +∞
    ```

    The partition is correct.

    Since the total length is odd:

    ```text
    median = max(leftA, leftB)
           = max(2, 1)
           = 2
    ```

    ## Complexity

    Let `m <= n`.

    **Time Complexity:**

    ```text
    O(log m)
    ```

    because binary search is performed only on the shorter array.

    **Space Complexity:**

    ```text
    O(1)
    ```

    No additional arrays are created.

    ## Key Points

    ```text
    i = number of nums1 elements in the left half

    j = number of nums2 elements in the left half

    i + j = (m + n + 1) // 2
    ```

    Correct partition:

    ```text
    leftA <= rightB
    leftB <= rightA
    ```

    Binary search direction:

    ```text
    leftA > rightB
    → i too large
    → move left

    leftB > rightA
    → i too small
    → move right
    ```

    Median:

    ```text
    Odd:
    max(leftA, leftB)

    Even:
    (max(leftA, leftB) + min(rightA, rightB)) / 2
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 7, 2026 |

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
