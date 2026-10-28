---
hide:
  - toc
---

# 392. Is Subsequence

*Edited by Charles*

**Easy · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/is-subsequence/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given two strings `s` and `t`, determine whether `s` is a subsequence of `t`.

    A **subsequence** is formed by deleting some (or no) characters from the original string without changing the relative order of the remaining characters.

## Python solution

```python
# Naive
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        n = len(s)
        m = len(t)
        if n == 0:
            return True

        ps = 0
        for i in t:
            if i == s[ps]:
                ps += 1
                if ps == len(s):
                    return True
        return False
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given two strings `s` and `t`, determine whether `s` is a subsequence of `t`.

    A **subsequence** is formed by deleting some (or no) characters from the original string without changing the relative order of the remaining characters.

    For example:

    ```python
    s = "abc"
    t = "ahbgdc"
    ```

    `"abc"` is a subsequence of `"ahbgdc"`, so the answer is `True`.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The length of the longest subsequence of `s[:i]` that can be matched within `t[:j]`.**
    >

    In other words, `dp[i][j]` represents how many characters from the first `i` characters of `s` have been successfully matched using the first `j` characters of `t`.

    ### Initialization

    If either string is empty, the longest matched subsequence has length `0`.

    Therefore, initialize the first row and first column to `0`.

    ```python
    dp = [[0] * (len(t) + 1) for _ in range(len(s) + 1)]
    ```

    ### State Transition

    If the current characters are equal,

    ```python
    s[i - 1] == t[j - 1]
    ```

    we can match this character and extend the previous matched subsequence.

    Update:

    ```python
    dp[i][j] = dp[i - 1][j - 1] + 1
    ```

    Otherwise,

    ```python
    dp[i][j] = dp[i][j - 1]
    ```

    Since we are checking whether `s` is a subsequence of `t`, when characters do not match, we simply skip the current character in `t` and continue searching.

    Transition equation:

    ```text
    if s[i - 1] == t[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + 1
    else:
        dp[i][j] = dp[i][j - 1]
    ```

    The key idea is:

    > If the current characters match, use both characters. Otherwise, skip the current character in `t`.
    >

    ### Final Answer

    If every character in `s` can be matched, then

    ```python
    dp[len(s)][len(t)] == len(s)
    ```

    which means `s` is a subsequence of `t`.

    Otherwise, return `False`.

    ## Example

    ```python
    s = "abc"
    t = "ahbgdc"
    ```

    Initialization:

    ```text
          a  h  b  g  d  c
       0  0  0  0  0  0  0
    a  0
    b  0
    c  0
    ```

    ### Match 'a'

    ```text
    a == a
    ```

    Update:

    ```text
    dp[1][1] = 1
    ```

    ### Match 'b'

    ```text
    b == b
    ```

    Update:

    ```text
    dp[2][3] = dp[1][2] + 1 = 2
    ```

    ### Match 'c'

    ```text
    c == c
    ```

    Update:

    ```text
    dp[3][6] = dp[2][5] + 1 = 3
    ```

    Final DP value:

    ```text
    dp[3][6] = 3
    ```

    Since

    ```python
    3 == len(s)
    ```

    the answer is

    ```python
    True
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    where

    - `m = len(s)`
    - `n = len(t)`

    Each state is computed exactly once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    A 2D DP table is required.

    ## Summary

    **State Definition**

    ```text
    dp[i][j] = length of the matched subsequence of s[:i] within t[:j]
    ```

    **Initialization**

    ```python
    dp = [[0] * (len(t) + 1) for _ in range(len(s) + 1)]
    ```

    **State Transition**

    ```python
    if s[i - 1] == t[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + 1
    else:
        dp[i][j] = dp[i][j - 1]
    ```

    **Traversal Order**

    Traverse both strings from left to right while filling the DP table.

    **Final Answer**

    ```python
    dp[len(s)][len(t)] == len(s)
    ```

    ## Common Mistakes

    1. This problem is **not** the standard Longest Common Subsequence (LCS) problem.
    2. When the current characters do not match, we only skip the current character in `t`:

    ```python
    dp[i][j] = dp[i][j - 1]
    ```

    instead of

    ```python
    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    because every character in `s` must be matched in order.

    1. The final answer is **not** `max(dp)`. Instead, check whether

    ```python
    dp[len(s)][len(t)] == len(s)
    ```

    If they are equal, then every character in `s` has been matched, so `s` is a subsequence of `t`.

    ## Codes

    ```python
    class Solution:
        def isSubsequence(self, s: str, t: str) -> bool:
            l1 = len(s)
            l2 = len(t)

            dp = [[0] * (l2 + 1) for _ in range(l1 + 1)]

            for i in range(1, l1 + 1):
                for j in range(1, l2 + 1):
                    if s[i - 1] == t[j - 1]:
                        dp[i][j] = dp[i - 1][j - 1] + 1
                    else:
                        dp[i][j] = dp[i][j - 1]

            if dp[-1][-1] == l1:
                return True

            return False
    ```

    ```python
    # Straighforward
    class Solution:
        def isSubsequence(self, s: str, t: str) -> bool:
            dp = [[False] * (len(t) + 1) for _ in range(len(s) + 1)]
            for i in range(len(t) + 1):
                dp[0][i] = True

            for i in range(1, len(s) + 1):
                for j in range(1, len(t) + 1):
    		            # 2 Cases
    		            # (1) s[i - 1] matches t[j - 1] and s[: i - 1] is a subsequence of t[: j - 1]
    		            # (2) s[i - 1] does not match t[j - 1], but s[: i] is already a subsequence of t[: j - 1]
                    dp[i][j] = dp[i][j - 1] or (dp[i - 1][j - 1] and s[i - 1] == t[j - 1])

            return dp[-1][-1]
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 38 Dynamic Programming XI](index.md) · [All Algorithm notes](../index.md)
