---
hide:
  - toc
---

# 300. Longest Increasing Subsequence

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/longest-increasing-subsequence/)

!!! tip "Core idea"

    Given an integer array `nums`, find the length of the **Longest Increasing Subsequence (LIS)**.

    Note that a **subsequence** does **not** need to be contiguous. It only needs to preserve the relative order of the elements.

## Python solution

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        l = len(nums)

        if l == 1:
            return 1

        dp = [1 for _ in range(l)]
        res = 1

        for i in range(1, l):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
            res = max(res, dp[i])

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given an integer array `nums`, find the length of the **Longest Increasing Subsequence (LIS)**.

    Note that a **subsequence** does **not** need to be contiguous. It only needs to preserve the relative order of the elements.

    For example:

    ```python
    nums = [10,9,2,5,3,7,101,18]
    ```

    One possible longest increasing subsequence is:

    ```python
    [2,3,7,101]
    ```

    Therefore, the answer is `4`.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i]
    ```

    denote:

    > **The length of the longest increasing subsequence ending at `nums[i]`.**
    >

    The phrase **"ending at `nums[i]`"** is very important.

    For example:

    ```python
    nums = [1,5,2,3]
    ```

    Then

    ```text
    dp[0] = 1    # [1]
    dp[1] = 2    # [1,5]
    dp[2] = 2    # [1,2]
    dp[3] = 3    # [1,2,3]
    ```

    ### Initialization

    Each element itself forms an increasing subsequence of length 1.

    Therefore,

    ```python
    dp = [1] * len(nums)
    ```

    That is,

    ```text
    dp[i] = 1
    ```

    ### State Transition

    For each position `i`, iterate through all previous positions `j`.

    If

    ```python
    nums[i] > nums[j]
    ```

    then `nums[i]` can be appended to the increasing subsequence ending at `nums[j]`.

    Update:

    ```python
    dp[i] = max(dp[i], dp[j] + 1)
    ```

    Transition equation:

    ```text
    dp[i] = max(dp[i], dp[j] + 1)
            where 0 <= j < i and nums[j] < nums[i]
    ```

    The idea is:

    > Find all previous elements that can precede `nums[i]`, choose the longest subsequence among them, and append `nums[i]`.
    >

    ### Why Can't We Return `dp[-1]`?

    The longest increasing subsequence does **not** necessarily end at the last element.

    For example,

    ```python
    nums = [1,2,3,0]
    ```

    Then

    ```text
    dp = [1,2,3,1]
    ```

    The LIS is

    ```python
    [1,2,3]
    ```

    with length `3`, while

    ```python
    dp[-1] = 1
    ```

    Therefore, the final answer should be

    ```python
    max(dp)
    ```

    or maintain a variable `res` to keep track of the global maximum during iteration.

    ## Example

    ```python
    nums = [4,10,4,3,8,9]
    ```

    Initialization:

    ```text
    nums = [4,10,4,3,8,9]
    dp   = [1,1,1,1,1,1]
    ```

    ### i = 1, nums[1] = 10

    Can be appended after:

    ```text
    4 -> 10
    ```

    Update:

    ```text
    dp = [1,2,1,1,1,1]
    ```

    ### i = 2, nums[2] = 4

    Previous elements:

    ```text
    4 > 4   False
    4 > 10  False
    ```

    No update:

    ```text
    dp = [1,2,1,1,1,1]
    ```

    ### i = 3, nums[3] = 3

    There is no previous element smaller than 3.

    ```text
    dp = [1,2,1,1,1,1]
    ```

    ### i = 4, nums[4] = 8

    Possible subsequences:

    ```text
    4 -> 8
    3 -> 8
    ```

    Update:

    ```text
    dp = [1,2,1,1,2,1]
    ```

    ### i = 5, nums[5] = 9

    Can be appended after:

    ```text
    4 -> 8 -> 9
    ```

    Update:

    ```text
    dp = [1,2,1,1,2,3]
    ```

    Final answer:

    ```python
    3
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n²)
    ```

    For each element, we iterate through all previous elements.

    **Space Complexity**

    ```text
    O(n)
    ```

    A DP array of length `n` is required.

    ## Summary

    **State Definition**

    ```text
    dp[i] = length of the longest increasing subsequence ending at nums[i]
    ```

    **Initialization**

    ```python
    dp = [1] * len(nums)
    ```

    **State Transition**

    ```python
    if nums[i] > nums[j]:
        dp[i] = max(dp[i], dp[j] + 1)
    ```

    **Traversal Order**

    Traverse the array from left to right. For each position, examine all previous elements.

    **Final Answer**

    ```python
    max(dp)
    ```

    ## Common Mistakes

    1. `dp[i]` represents the LIS **ending at** `nums[i]`, **not** the LIS within the first `i` elements.
    2. Do **not** return `dp[-1]`, since the LIS does not necessarily end at the last element.
    3. The sequence must be **strictly increasing**, so the condition must be

    ```python
    nums[i] > nums[j]
    ```

    instead of

    ```python
    nums[i] >= nums[j]
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 9, 2026 |

---

[Back to Day 37 Dynamic Programming X](index.md) · [All Algorithm notes](../index.md)
