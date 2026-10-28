---
hide:
  - toc
---

# 1143. Longest Common Subsequence

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/longest-common-subsequence/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given two strings `text1` and `text2`, return the length of their **Longest Common Subsequence (LCS)**.

    A **subsequence** is obtained by deleting some (or no) characters from a string without changing the relative order of the remaining characters.

## Python solution

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        l1 = len(text1)
        l2 = len(text2)

        dp = [[0] * (l2 + 1) for _ in range(l1 + 1)]

        for i in range(1, l1 + 1):
            for j in range(1, l2 + 1):
                if text1[i - 1] == text2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

        return dp[-1][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given two strings `text1` and `text2`, return the length of their **Longest Common Subsequence (LCS)**.

    A **subsequence** is obtained by deleting some (or no) characters from a string without changing the relative order of the remaining characters.

    Unlike the **Longest Common Subarray** problem, the matched characters **do not need to be contiguous**.

    For example:

    ```python
    text1 = "abcde"
    text2 = "ace"
    ```

    The longest common subsequence is:

    ```python
    "ace"
    ```

    Therefore, the answer is `3`.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The length of the longest common subsequence between `text1[:i]` and `text2[:j]`.**
    >

    Unlike the Longest Common Subarray problem, `dp[i][j]` does **not** require the subsequence to end at `text1[i-1]` and `text2[j-1]`.

    ### Initialization

    If either string is empty, there is no common subsequence.

    Therefore, initialize the first row and first column to `0`.

    ```python
    dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
    ```

    The extra row and column eliminate boundary checks.

    ### State Transition

    #### Case 1: Current characters match

    If

    ```python
    text1[i - 1] == text2[j - 1]
    ```

    the current characters can be included in the longest common subsequence.

    Update:

    ```python
    dp[i][j] = dp[i - 1][j - 1] + 1
    ```

    #### Case 2: Current characters do not match

    If the current characters are different, at least one of them cannot belong to the LCS.

    There are two possibilities:

    - Ignore the current character in `text1`
    - Ignore the current character in `text2`

    Take the better result:

    ```python
    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    Transition equation:

    ```text
    if text1[i - 1] == text2[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + 1
    else:
        dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    The key idea is:

    > If the current characters match, include them in the LCS. Otherwise, discard one character and keep the longer subsequence.
    >

    ### Final Answer

    Since `dp[i][j]` represents the LCS length between the prefixes `text1[:i]` and `text2[:j]`, the final answer is

    ```python
    dp[len(text1)][len(text2)]
    ```

    which is equivalent to

    ```python
    dp[-1][-1]
    ```

    ## Example

    ```python
    text1 = "abcde"
    text2 = "ace"
    ```

    Initialization:

    ```text
          a  c  e
       0  0  0  0
    a  0
    b  0
    c  0
    d  0
    e  0
    ```

    ### Match 'a'

    ```text
    a == a
    ```

    Update:

    ```text
    dp[1][1] = 1
    ```

    ### Compare 'b' and 'c'

    ```text
    b != c
    ```

    Choose the larger value:

    ```text
    dp[2][2] = max(dp[1][2], dp[2][1]) = 1
    ```

    ### Match 'c'

    ```text
    c == c
    ```

    Update:

    ```text
    dp[3][2] = dp[2][1] + 1 = 2
    ```

    ### Match 'e'

    ```text
    e == e
    ```

    Update:

    ```text
    dp[5][3] = dp[4][2] + 1 = 3
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

    - `m = len(text1)`
    - `n = len(text2)`

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
    dp[i][j] = length of the longest common subsequence between text1[:i] and text2[:j]
    ```

    **Initialization**

    ```python
    dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
    ```

    **State Transition**

    ```python
    if text1[i - 1] == text2[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + 1
    else:
        dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    **Traversal Order**

    Traverse both strings from left to right while filling the DP table.

    **Final Answer**

    ```python
    dp[-1][-1]
    ```

    ## Common Mistakes

    1. Do **not** confuse this problem with **Longest Common Subarray (LeetCode 718)**.
        - **Longest Common Subarray:** elements must be **contiguous**.
        - **Longest Common Subsequence:** elements only need to preserve their relative order.
    2. When the current characters do not match, do **not** reset the state to `0`.

        Instead,


    ```python
    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    because previously matched subsequences can still be extended later.

    1. `dp[i][j]` represents the LCS length between two prefixes, **not** the LCS ending at the current characters.
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

---

[Back to Day 38 Dynamic Programming XI](index.md) · [All Algorithm notes](../index.md)
