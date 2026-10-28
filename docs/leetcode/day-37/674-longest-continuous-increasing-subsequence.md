---
hide:
  - toc
---

# 674. Longest Continuous Increasing Subsequence

*Edited by Charles*

**Easy · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)

!!! tip "Core idea"

    Given an integer array `nums`, find the length of the **Longest Continuous Increasing Subsequence (LCIS)**.

    Unlike **Longest Increasing Subsequence (LIS)**, the elements in LCIS **must be contiguous**.

## Python solution

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        l = len(nums)

        dp = [1] * l
        res = 1

        for i in range(1, l):
            if nums[i] > nums[i - 1]:
                dp[i] = dp[i - 1] + 1
            res = max(res, dp[i])

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given an integer array `nums`, find the length of the **Longest Continuous Increasing Subsequence (LCIS)**.

    Unlike **Longest Increasing Subsequence (LIS)**, the elements in LCIS **must be contiguous**.

    For example:

    ```python
    nums = [1,3,5,4,7]
    ```

    The longest continuous increasing subsequence is:

    ```python
    [1,3,5]
    ```

    Its length is `3`.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i]
    ```

    denote:

    > **The length of the longest continuous increasing subsequence ending at `nums[i]`.**
    >

    Since the subsequence must be **continuous**, the previous element can only be `nums[i-1]`.

    For example:

    ```python
    nums = [1,3,5,4,7]
    ```

    Then

    ```text
    dp[0] = 1    # [1]
    dp[1] = 2    # [1,3]
    dp[2] = 3    # [1,3,5]
    dp[3] = 1    # [4]
    dp[4] = 2    # [4,7]
    ```

    ### Initialization

    Each element itself forms a continuous increasing subsequence of length `1`.

    Therefore,

    ```python
    dp = [1] * len(nums)
    ```

    That is,

    ```text
    dp[i] = 1
    ```

    ### State Transition

    Since the subsequence must be **continuous**, we only need to compare the current element with its immediate previous element.

    If

    ```python
    nums[i] > nums[i - 1]
    ```

    then `nums[i]` can extend the previous continuous increasing subsequence.

    Update:

    ```python
    dp[i] = dp[i - 1] + 1
    ```

    Otherwise,

    ```python
    dp[i] = 1
    ```

    since a new subsequence starts at `nums[i]`.

    Transition equation:

    ```text
    if nums[i] > nums[i - 1]:
        dp[i] = dp[i - 1] + 1
    else:
        dp[i] = 1
    ```

    The key observation is that continuity removes the need to examine all previous elements.

    ### Why Do We Need a Global Maximum?

    The longest continuous increasing subsequence does **not** necessarily end at the last element.

    For example,

    ```python
    nums = [1,2,3,0]
    ```

    Then

    ```text
    dp = [1,2,3,1]
    ```

    The answer is

    ```python
    3
    ```

    instead of

    ```python
    dp[-1] = 1
    ```

    Therefore, we maintain a variable `res` to record the maximum value throughout the traversal.

    ## Example

    ```python
    nums = [1,3,5,4,7]
    ```

    Initialization:

    ```text
    nums = [1,3,5,4,7]
    dp   = [1,1,1,1,1]
    ```

    ### i = 1, nums[1] = 3

    ```text
    3 > 1
    ```

    Extend the previous subsequence:

    ```text
    dp = [1,2,1,1,1]
    ```

    ### i = 2, nums[2] = 5

    ```text
    5 > 3
    ```

    Continue extending:

    ```text
    dp = [1,2,3,1,1]
    ```

    ### i = 3, nums[3] = 4

    ```text
    4 > 5   False
    ```

    The increasing sequence is broken.

    Start a new subsequence:

    ```text
    dp = [1,2,3,1,1]
    ```

    ### i = 4, nums[4] = 7

    ```text
    7 > 4
    ```

    Extend the new subsequence:

    ```text
    dp = [1,2,3,1,2]
    ```

    Final answer:

    ```python
    3
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n)
    ```

    Each element is visited exactly once.

    **Space Complexity**

    ```text
    O(n)
    ```

    A DP array of length `n` is used.

    > **Note:** Since `dp[i]` only depends on `dp[i-1]`, the space complexity can be optimized to **O(1)** by keeping only the current length.
    >

    ## Summary

    **State Definition**

    ```text
    dp[i] = length of the longest continuous increasing subsequence ending at nums[i]
    ```

    **Initialization**

    ```python
    dp = [1] * len(nums)
    ```

    **State Transition**

    ```python
    if nums[i] > nums[i - 1]:
        dp[i] = dp[i - 1] + 1
    else:
        dp[i] = 1
    ```

    **Traversal Order**

    Traverse the array from left to right, comparing each element with its immediate predecessor.

    **Final Answer**

    ```python
    max(dp)
    ```

    or maintain a running maximum during traversal.

    ## Common Mistakes

    1. This problem asks for a **continuous** increasing subsequence, so elements **must be adjacent**.
    2. Unlike LeetCode 300 (Longest Increasing Subsequence), there is **no need to iterate through all previous elements**. Only compare `nums[i]` with `nums[i - 1]`.
    3. Do **not** return `dp[-1]`, since the longest continuous increasing subsequence does not necessarily end at the last element.
    4. The sequence must be **strictly increasing**, so the condition should be

    ```python
    nums[i] > nums[i - 1]
    ```

    instead of

    ```python
    nums[i] >= nums[i - 1]
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 37 Dynamic Programming X](index.md) · [All LeetCode notes](../index.md)
