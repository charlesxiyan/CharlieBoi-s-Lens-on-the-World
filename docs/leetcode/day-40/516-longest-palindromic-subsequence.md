---
hide:
  - toc
---

# 516. Longest Palindromic Subsequence

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/longest-palindromic-subsequence/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given a string `s`, return the length of the **longest palindromic subsequence**.

    A **subsequence** does not need to be contiguous, but the relative order of the selected characters must remain unchanged.

## Python solution

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        l = len(s)
        dp = [[0] * l for _ in range(l)]
        for i in range(l):
            dp[i][i] = 1

        for i in range(l - 1, -1, -1):
            for j in range(i + 1, l):
                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

        return dp[0][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given a string `s`, return the length of the **longest palindromic subsequence**.

    A **subsequence** does not need to be contiguous, but the relative order of the selected characters must remain unchanged.

    For example:

    ```python
    s = "bbbab"
    ```

    One longest palindromic subsequence is:

    ```text
    "bbbb"
    ```

    Therefore, the answer is `4`.

    The key observation is that when considering a substring `s[i:j+1]`, the result depends on whether the two boundary characters `s[i]` and `s[j]` are equal.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The length of the longest palindromic subsequence inside the substring `s[i:j+1]`.**
    >

    The interval is inclusive on both sides:

    ```text
    [i, j]
    ```

    So `dp[i][j]` considers every possible subsequence inside this range.

    ### Initialization

    When

    ```text
    i == j
    ```

    the substring contains only one character.

    A single character is always a palindrome of length `1`.

    Therefore:

    ```python
    dp[i][i] = 1
    ```

    All other states can initially remain `0`.

    ### State Transition

    There are two cases.

    #### Case 1: `s[i] == s[j]`

    If the two boundary characters are equal, they can both be added to the longest palindromic subsequence inside the interval:

    ```text
    [i + 1, j - 1]
    ```

    Therefore:

    ```python
    dp[i][j] = dp[i + 1][j - 1] + 2
    ```

    For example:

    ```text
    s[i:j+1] = "cabac"
    ```

    Since:

    ```text
    s[i] == s[j] == 'c'
    ```

    we only need to know the longest palindromic subsequence inside:

    ```text
    "aba"
    ```

    and then add the two `'c'` characters.

    #### Case 2: `s[i] != s[j]`

    If the two boundary characters are different, they cannot both be included as matching ends of the same palindrome.

    We consider two possibilities:

    - Ignore `s[i]`
    - Ignore `s[j]`

    If we ignore `s[i]`, the result is:

    ```python
    dp[i + 1][j]
    ```

    If we ignore `s[j]`, the result is:

    ```python
    dp[i][j - 1]
    ```

    Take the larger value:

    ```python
    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    ```

    ### Transition Equation

    ```python
    if s[i] == s[j]:
        dp[i][j] = dp[i + 1][j - 1] + 2
    else:
        dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    ```

    ### Traversal Order

    The transition depends on:

    ```text
    dp[i + 1][j - 1]
    dp[i + 1][j]
    dp[i][j - 1]
    ```

    Therefore, before computing `dp[i][j]`:

    - the next row `i + 1` must already be computed;
    - the value to the left `dp[i][j - 1]` must already be computed.

    So `i` must be traversed from bottom to top:

    ```python
    for i in range(len(s) - 1, -1, -1):
    ```

    and `j` should move from left to right:

    ```python
    for j in range(i + 1, len(s)):
    ```

    Traversal order:

    ```text
    i: right to left / bottom to top
    j: left to right
    ```

    ### Final Answer

    The state:

    ```python
    dp[0][len(s) - 1]
    ```

    represents the longest palindromic subsequence inside the entire string.

    Therefore, the final answer is:

    ```python
    dp[0][-1]
    ```

    ## Example

    ```python
    s = "cbbd"
    ```

    Initialization:

    ```text
    dp[0][0] = 1   # "c"
    dp[1][1] = 1   # "b"
    dp[2][2] = 1   # "b"
    dp[3][3] = 1   # "d"
    ```

    ### Compare `s[2]` and `s[3]`

    ```text
    'b' != 'd'
    ```

    Therefore:

    ```text
    dp[2][3] = max(dp[3][3], dp[2][2])
             = 1
    ```

    ### Compare `s[1]` and `s[2]`

    ```text
    'b' == 'b'
    ```

    Therefore:

    ```text
    dp[1][2] = dp[2][1] + 2
             = 2
    ```

    The palindrome is:

    ```text
    "bb"
    ```

    ### Compare `s[1]` and `s[3]`

    ```text
    'b' != 'd'
    ```

    Therefore:

    ```text
    dp[1][3] = max(dp[2][3], dp[1][2])
             = 2
    ```

    ### Compare `s[0]` and `s[3]`

    ```text
    'c' != 'd'
    ```

    Therefore:

    ```text
    dp[0][3] = max(dp[1][3], dp[0][2])
             = 2
    ```

    Final answer:

    ```python
    2
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n²)
    ```

    We compute every valid interval `[i, j]`.

    **Space Complexity**

    ```text
    O(n²)
    ```

    A 2D DP table is used.

    ## Summary

    **State Definition**

    ```text
    dp[i][j] = length of the longest palindromic subsequence inside s[i:j+1]
    ```

    **Initialization**

    ```python
    dp[i][i] = 1
    ```

    **State Transition**

    ```python
    if s[i] == s[j]:
        dp[i][j] = dp[i + 1][j - 1] + 2
    else:
        dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    ```

    **Traversal Order**

    ```text
    i: from right to left
    j: from i + 1 to right
    ```

    **Final Answer**

    ```python
    dp[0][-1]
    ```

    ## Common Mistakes

    1. Do not confuse **palindromic subsequence** with **palindromic substring**.

    ```text
    Substring   -> must be contiguous
    Subsequence -> does not need to be contiguous
    ```

    1. When:

    ```python
    s[i] == s[j]
    ```

    both boundary characters can be included:

    ```python
    dp[i][j] = dp[i + 1][j - 1] + 2
    ```

    1. When:

    ```python
    s[i] != s[j]
    ```

    do not reset the state to `0`.

    Instead, discard one boundary and take the better result:

    ```python
    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    ```

    1. The traversal order is important because `dp[i][j]` depends on states from the next row. Therefore, `i` must be traversed from right to left.
    2. Every single character is a palindrome of length `1`, so the diagonal must be initialized as:

    ```python
    dp[i][i] = 1
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 12, 2026 |

---

[Back to Day 40 Dynamic Programming XIII](index.md) · [All Algorithm notes](../index.md)
