---
hide:
  - toc
---

# 647. Palindromic Substrings

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/palindromic-substrings/)

!!! tip "Core idea"

    Given a string `s`, return the total number of **palindromic substrings** in `s`.

    A palindrome reads the same forward and backward.

## Python solution

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        l = len(s)

        dp = [[False] * l for _ in range(l)]
        res = 0

        for i in range(l - 1, -1, -1):
            for j in range(i, l):
                if s[i] == s[j]:
                    if j - i <= 1:
                        res += 1
                        dp[i][j] = True
                    elif dp[i + 1][j - 1]:
                        res += 1
                        dp[i][j] = True

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given a string `s`, return the total number of **palindromic substrings** in `s`.

    A palindrome reads the same forward and backward.

    For example:

    ```python
    s = "aaa"
    ```

    The palindromic substrings are:

    ```text
    "a"
    "a"
    "a"
    "aa"
    "aa"
    "aaa"
    ```

    Therefore, the answer is `6`.

    This problem is different from many previous subsequence DP problems because defining something like

    ```text
    dp[i] = number of palindromes ending at i
    ```

    does not give us a clear recurrence.

    Instead, we use the key property of a palindrome:

    > A substring `s[i:j+1]` is a palindrome if its two end characters are equal and its inner substring `s[i+1:j]` is also a palindrome.
    >

    This naturally leads to a 2D DP solution.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **Whether the substring `s[i:j+1]` is a palindrome.**
    >

    The interval is inclusive on both sides:

    ```text
    [i, j]
    ```

    So:

    ```text
    dp[i][j] = True
    ```

    means `s[i:j+1]` is a palindromic substring.

    ### Initialization

    Initially, set every state to `False`.

    ```python
    dp = [[False] * len(s) for _ in range(len(s))]
    ```

    We should not assume any substring is a palindrome before checking it.

    ### State Transition

    There are two main cases.

    #### Case 1: `s[i] != s[j]`

    If the two end characters are different, the substring cannot be a palindrome.

    Therefore:

    ```python
    dp[i][j] = False
    ```

    Since the DP table is initialized to `False`, no update is needed.

    #### Case 2: `s[i] == s[j]`

    If the two end characters are equal, there are three situations.

    ### Situation 1: Single Character

    If

    ```text
    i == j
    ```

    then the substring contains only one character.

    Every single character is a palindrome.

    For example:

    ```text
    "a"
    ```

    ### Situation 2: Two Characters

    If

    ```text
    j - i == 1
    ```

    and

    ```python
    s[i] == s[j]
    ```

    then the substring is also a palindrome.

    For example:

    ```text
    "aa"
    ```

    ### Situation 3: Length Greater Than 2

    If

    ```text
    j - i > 1
    ```

    and the two end characters are equal, then we only need to check whether the inner substring is a palindrome.

    That is:

    ```python
    dp[i + 1][j - 1]
    ```

    So:

    ```python
    dp[i][j] = dp[i + 1][j - 1]
    ```

    when

    ```python
    s[i] == s[j]
    ```

    ### Combined Transition

    ```python
    if s[i] == s[j]:
        if j - i <= 1:
            dp[i][j] = True
        elif dp[i + 1][j - 1]:
            dp[i][j] = True
    ```

    Whenever `dp[i][j]` becomes `True`, we have found one palindromic substring, so increment the result.

    ```python
    res += 1
    ```

    ### Why Is `j - i <= 1` a Special Case?

    Consider:

    ```text
    i == j
    ```

    The substring has length `1`, so it is automatically a palindrome.

    Now consider:

    ```text
    j - i == 1
    ```

    The substring has length `2`.

    If:

    ```python
    s[i] == s[j]
    ```

    then it is also automatically a palindrome.

    For these cases, there is no meaningful inner substring `s[i+1:j]` to check.

    Therefore:

    ```python
    j - i <= 1
    ```

    is handled directly.

    ### Traversal Order

    This is the most important part of this DP solution.

    For longer substrings, we use:

    ```python
    dp[i + 1][j - 1]
    ```

    to calculate:

    ```python
    dp[i][j]
    ```

    So before computing `dp[i][j]`, the state

    ```python
    dp[i + 1][j - 1]
    ```

    must already be known.

    Notice that `dp[i + 1][j - 1]` lies **below and to the left** of `dp[i][j]` in the DP table.

    Therefore, `i` must be traversed **from bottom to top**:

    ```python
    for i in range(len(s) - 1, -1, -1):
    ```

    For each `i`, `j` moves from left to right:

    ```python
    for j in range(i, len(s)):
    ```

    Traversal order:

    ```text
    i: bottom -> top
    j: left -> right
    ```

    This guarantees that `dp[i + 1][j - 1]` has already been computed.

    ## Example

    ```python
    s = "aaa"
    ```

    The DP table corresponds to substrings `s[i:j+1]`.

    ### i = 2

    ```text
    s[2:3] = "a"
    ```

    Single character:

    ```text
    dp[2][2] = True
    ```

    Count:

    ```text
    res = 1
    ```

    ### i = 1

    For:

    ```text
    s[1:2] = "a"
    ```

    we get:

    ```text
    dp[1][1] = True
    ```

    For:

    ```text
    s[1:3] = "aa"
    ```

    since:

    ```text
    s[1] == s[2]
    ```

    and the length is `2`:

    ```text
    dp[1][2] = True
    ```

    Now:

    ```text
    res = 3
    ```

    ### i = 0

    For:

    ```text
    s[0:1] = "a"
    ```

    ```text
    dp[0][0] = True
    ```

    For:

    ```text
    s[0:2] = "aa"
    ```

    ```text
    dp[0][1] = True
    ```

    For:

    ```text
    s[0:3] = "aaa"
    ```

    the end characters are equal:

    ```text
    s[0] == s[2]
    ```

    and the inner substring:

    ```text
    s[1:2] = "a"
    ```

    is a palindrome because:

    ```text
    dp[1][1] = True
    ```

    Therefore:

    ```text
    dp[0][2] = True
    ```

    Final result:

    ```python
    6
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n²)
    ```

    We examine every possible substring interval `[i, j]`.

    **Space Complexity**

    ```text
    O(n²)
    ```

    A 2D DP table is used.

    ## Summary

    **State Definition**

    ```text
    dp[i][j] = whether s[i:j+1] is a palindrome
    ```

    **Initialization**

    ```python
    dp = [[False] * len(s) for _ in range(len(s))]
    ```

    **State Transition**

    ```python
    if s[i] == s[j]:
        if j - i <= 1:
            dp[i][j] = True
        elif dp[i + 1][j - 1]:
            dp[i][j] = True
    ```

    Whenever:

    ```python
    dp[i][j] == True
    ```

    increment the palindrome count.

    **Traversal Order**

    ```text
    i: from right to left
    j: from i to right
    ```

    **Final Answer**

    ```text
    Total number of states dp[i][j] that become True
    ```

    ## Common Mistakes

    1. Do not define the DP state only using one index.

        The palindrome property depends on both ends of a substring, so a 2D state is much more natural:


    ```text
    dp[i][j]
    ```

    1. Do not forget the special cases:

    ```text
    j - i <= 1
    ```

    These represent substrings of length `1` or `2`.

    1. When the substring length is greater than `2`, checking only

    ```python
    s[i] == s[j]
    ```

    is not enough.

    You must also check:

    ```python
    dp[i + 1][j - 1]
    ```

    1. The traversal order is crucial.

    Since:

    ```python
    dp[i][j]
    ```

    depends on:

    ```python
    dp[i + 1][j - 1]
    ```

    `i` must be traversed from right to left.

    1. A substring must be **continuous**.

    This problem counts palindromic **substrings**, not palindromic subsequences.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 12, 2026 |

---

[Back to Day 40 Dynamic Programming XIII](index.md) · [All LeetCode notes](../index.md)
