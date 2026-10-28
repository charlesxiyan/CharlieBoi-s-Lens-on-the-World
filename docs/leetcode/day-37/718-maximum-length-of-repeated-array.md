---
hide:
  - toc
---

# 718. Maximum Length of Repeated Array

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given two integer arrays `nums1` and `nums2`, return the length of the longest common **contiguous** subarray.

    Unlike the **Longest Common Subsequence (LCS)** problem, the matching elements in this problem **must be contiguous** in both arrays.

## Python solution

```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        l1 = len(nums1)
        l2 = len(nums2)

        dp = [[0] * (l2 + 1) for _ in range(l1 + 1)]
        res = 0

        for i in range(1, l1 + 1):
            for j in range(1, l2 + 1):
                if nums1[i - 1] == nums2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                res = max(res, dp[i][j])

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given two integer arrays `nums1` and `nums2`, return the length of the longest common **contiguous** subarray.

    Unlike the **Longest Common Subsequence (LCS)** problem, the matching elements in this problem **must be contiguous** in both arrays.

    For example:

    ```python
    nums1 = [1,2,3,2,1]
    nums2 = [3,2,1,4,7]
    ```

    The longest common subarray is:

    ```python
    [3,2,1]
    ```

    Therefore, the answer is `3`.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The length of the longest common subarray ending at `nums1[i - 1]` and `nums2[j - 1]`.**
    >

    The keyword **"ending at"** is crucial because the subarray must be continuous.

    For example:

    ```python
    nums1 = [1,2,3]
    nums2 = [2,3]
    ```

    The DP table will eventually contain:

    ```text
    dp[2][1] = 1    # [2]
    dp[3][2] = 2    # [2,3]
    ```

    ### Initialization

    If either array is empty, there is no common subarray.

    Therefore, initialize the first row and first column to `0`.

    ```python
    dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
    ```

    The extra row and column eliminate boundary checks when accessing `dp[i - 1][j - 1]`.

    ### State Transition

    If the current elements are equal,

    ```python
    nums1[i - 1] == nums2[j - 1]
    ```

    then the common subarray can be extended by one element.

    Update:

    ```python
    dp[i][j] = dp[i - 1][j - 1] + 1
    ```

    Otherwise,

    ```python
    dp[i][j] = 0
    ```

    because the subarray must be **continuous**, and a mismatch breaks the current subarray.

    Transition equation:

    ```text
    if nums1[i - 1] == nums2[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + 1
    else:
        dp[i][j] = 0
    ```

    The key idea is:

    > A common subarray can only be extended when the current elements are equal. Otherwise, the current common subarray ends immediately.
    >

    ### Why Can't We Return `dp[-1][-1]`?

    The longest common subarray does **not** necessarily end at the last elements of both arrays.

    For example,

    ```python
    nums1 = [1,2,3,4]
    nums2 = [2,3,5]
    ```

    The DP table contains:

    ```text
    ...
    2
    0
    ```

    The longest common subarray is

    ```python
    [2,3]
    ```

    with length `2`, but

    ```python
    dp[-1][-1] = 0
    ```

    Therefore, we must maintain a global maximum throughout the DP process.

    ## Example

    ```python
    nums1 = [1,2,3,2,1]
    nums2 = [3,2,1,4,7]
    ```

    Initialization:

    ```text
          3  2  1  4  7
       0  0  0  0  0  0
    1  0
    2  0
    3  0
    2  0
    1  0
    ```

    ### Match: nums1[2] == nums2[0]

    ```text
    3 == 3
    ```

    Update:

    ```text
    dp[3][1] = dp[2][0] + 1 = 1
    ```

    ### Match: nums1[3] == nums2[1]

    ```text
    2 == 2
    ```

    Update:

    ```text
    dp[4][2] = dp[3][1] + 1 = 2
    ```

    ### Match: nums1[4] == nums2[2]

    ```text
    1 == 1
    ```

    Update:

    ```text
    dp[5][3] = dp[4][2] + 1 = 3
    ```

    The longest common subarray becomes

    ```text
    [3,2,1]
    ```

    Final answer:

    ```python
    3
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    where

    - `m = len(nums1)`
    - `n = len(nums2)`

    Each pair of elements is processed exactly once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    A 2D DP table is required.

    > **Note:** Since each state only depends on the upper-left diagonal element, the DP table can be optimized to **O(n)** using a rolling array.
    >

    ## Summary

    **State Definition**

    ```text
    dp[i][j] = length of the longest common subarray ending at nums1[i-1] and nums2[j-1]
    ```

    **Initialization**

    ```python
    dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
    ```

    **State Transition**

    ```python
    if nums1[i - 1] == nums2[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + 1
    else:
        dp[i][j] = 0
    ```

    **Traversal Order**

    Traverse both arrays from left to right while filling the DP table.

    **Final Answer**

    Maintain a global maximum during the DP process.

    ## Common Mistakes

    1. This problem asks for the **Longest Common Subarray**, **not** the Longest Common Subsequence (LCS). The matched elements **must be contiguous**.
    2. `dp[i][j]` represents the longest common subarray **ending at** `nums1[i-1]` and `nums2[j-1]`.
    3. When two elements are different,

    ```python
    dp[i][j] = 0
    ```

    instead of taking the maximum of neighboring states, because the current common subarray is broken.

    1. Do **not** return `dp[-1][-1]`, since the longest common subarray may end anywhere in the two arrays. Maintain a global maximum instead.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 37 Dynamic Programming X](index.md) · [All Algorithm notes](../index.md)
