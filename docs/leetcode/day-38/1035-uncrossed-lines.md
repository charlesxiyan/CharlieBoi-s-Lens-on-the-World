---
hide:
  - toc
---

# 1035. Uncrossed Lines

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/uncrossed-lines/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given two integer arrays `nums1` and `nums2`, return the maximum number of connecting lines that can be drawn between equal numbers such that no two lines intersect.

        Although the problem is described in terms of drawing lines, it is essentially asking for the **Longest Common Subsequence (LCS)** of the two arrays.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given two integer arrays `nums1` and `nums2`, return the maximum number of connecting lines that can be drawn between equal numbers such that no two lines intersect.

    Although the problem is described in terms of drawing lines, it is essentially asking for the **Longest Common Subsequence (LCS)** of the two arrays.

    The reason is that once two matching elements are connected, their relative order must be preserved to avoid crossing lines, which is exactly the definition of a subsequence.

    For example:

    ```python
    nums1 = [1,4,2]
    nums2 = [1,2,4]
    ```

    The maximum number of uncrossed lines is:

    ```text
    2
    ```

    One possible matching is:

    ```text
    1 ---- 1
    2 ---- 2
    ```

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The maximum number of uncrossed lines that can be drawn between `nums1[:i]` and `nums2[:j]`.**
    >

    Equivalently,

    > **The length of the Longest Common Subsequence (LCS) between `nums1[:i]` and `nums2[:j]`.**
    >

    ### Initialization

    If either array is empty, no lines can be drawn.

    Therefore, initialize the first row and first column to `0`.

    ```python
    dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
    ```

    The extra row and column eliminate boundary checks.

    ### State Transition

    #### Case 1: Current elements match

    If

    ```python
    nums1[i - 1] == nums2[j - 1]
    ```

    the two elements can be connected without crossing previous lines.

    Update:

    ```python
    dp[i][j] = dp[i - 1][j - 1] + 1
    ```

    #### Case 2: Current elements do not match

    If the current elements are different, one of them cannot participate in the optimal solution.

    There are two possibilities:

    - Ignore the current element in `nums1`
    - Ignore the current element in `nums2`

    Take the better result:

    ```python
    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    Transition equation:

    ```text
    if nums1[i - 1] == nums2[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + 1
    else:
        dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    The key idea is:

    > Connecting two equal numbers is equivalent to selecting one character in the Longest Common Subsequence problem.
    >

    ### Final Answer

    Since `dp[i][j]` represents the maximum number of uncrossed lines between the prefixes of the two arrays, the final answer is

    ```python
    dp[len(nums1)][len(nums2)]
    ```

    which is equivalent to

    ```python
    dp[-1][-1]
    ```

    ## Example

    ```python
    nums1 = [1,4,2]
    nums2 = [1,2,4]
    ```

    Initialization:

    ```text
          1  2  4
       0  0  0  0
    1  0
    4  0
    2  0
    ```

    ### Match 1

    ```text
    1 == 1
    ```

    Update:

    ```text
    dp[1][1] = 1
    ```

    ### Compare 4 and 2

    ```text
    4 != 2
    ```

    Update:

    ```text
    dp[2][2] = max(dp[1][2], dp[2][1]) = 1
    ```

    ### Match 4

    ```text
    4 == 4
    ```

    Update:

    ```text
    dp[2][3] = dp[1][2] + 1 = 2
    ```

    ### Compare 2 and 4

    ```text
    2 != 4
    ```

    Update:

    ```text
    dp[3][3] = max(dp[2][3], dp[3][2]) = 2
    ```

    Final answer:

    ```python
    2
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    where

    - `m = len(nums1)`
    - `n = len(nums2)`

    Each state is computed exactly once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    A 2D DP table is required.

    > **Note:** The DP table can be optimized to **O(n)** using a rolling array because each state only depends on the previous row and the current row.
    >

    ## Summary

    **State Definition**

    ```text
    dp[i][j] = maximum number of uncrossed lines between nums1[:i] and nums2[:j]
    ```

    or equivalently,

    ```text
    dp[i][j] = length of the Longest Common Subsequence between nums1[:i] and nums2[:j]
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
        dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    **Traversal Order**

    Traverse both arrays from left to right while filling the DP table.

    **Final Answer**

    ```python
    dp[-1][-1]
    ```

    ## Common Mistakes

    1. Although the problem describes drawing lines, it is **exactly the Longest Common Subsequence (LCS) problem**.
    2. Since the lines cannot cross, the matched elements must preserve their relative order.
    3. When the current elements do not match, do **not** reset the state to `0`.

    Instead,

    ```python
    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    because previous matches are still valid.

    1. `dp[i][j]` represents the optimal solution for the prefixes of the two arrays, **not** the solution ending at the current elements.
    2. The answer is directly stored in

    ```python
    dp[-1][-1]
    ```

    There is no need to maintain a separate global maximum.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

## Python solution

```python
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        l1 = len(nums1)
        l2 = len(nums2)

        dp = [[0] * (l2 + 1) for _ in range(l1 + 1)]

        for i in range(1, l1 + 1):
            for j in range(1, l2 + 1):
                if nums1[i - 1] == nums2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

        return dp[-1][-1]
```

---

[Back to Day 38 Dynamic Programming XI](index.md) · [All Algorithm notes](../index.md)
