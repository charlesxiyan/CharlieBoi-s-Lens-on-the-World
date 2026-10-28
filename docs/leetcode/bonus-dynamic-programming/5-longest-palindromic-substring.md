---
hide:
  - toc
---

# 5. Longest Palindromic Substring

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/longest-palindromic-substring/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given a string `s`, return the **longest palindromic substring** in `s`.

    A palindrome is a string that reads the same forward and backward.

## Python solution

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)

        dp = [[False] * n for _ in range(n)]

        start = 0
        maxLength = 1

        for i in range(n - 1, -1, -1):
            for j in range(i, n):
                if s[i] == s[j] and (j - i <= 1 or dp[i + 1][j - 1]):
                    dp[i][j] = True

                    if j - i + 1 > maxLength:
                        maxLength = j - i + 1
                        start = i

        return s[start:start + maxLength]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given a string `s`, return the **longest palindromic substring** in `s`.

    A palindrome is a string that reads the same forward and backward.

    For example:

    ```text
    "aba"   → palindrome
    "abba"  → palindrome
    "abc"   → not palindrome
    ```

    Important:

    > This problem asks for a **substring**, so all characters must be continuous.
    >

    ```text
    Input: s = "babad"

    Output: "bab"
    ```

    `"aba"` is also a valid answer.

    Another example:

    ```text
    Input: s = "cbbd"

    Output: "bb"
    ```

    The key question is:

    > How can we determine whether `s[i:j+1]` is a palindrome?
    >

    Consider:

    ```text
    s[i] ........ s[j]
    ```

    For this substring to be a palindrome, two conditions must hold:

    ```text
    1. s[i] == s[j]
    2. The substring inside s[i+1:j] is also a palindrome
    ```

    This naturally leads to Dynamic Programming.

    Define:

    ```python
    dp[i][j]
    ```

    as:

    > Whether the substring `s[i:j+1]` is a palindrome.
    >

    Therefore:

    ```text
    dp[i][j] = True
    ```

    means:

    ```text
    s[i:j+1]
    ```

    is a palindrome.

    For example:

    ```text
    s = "babad"
    ```

    If:

    ```text
    i = 0
    j = 2
    ```

    then:

    ```text
    s[i:j+1] = "bab"
    ```

    so:

    ```text
    dp[0][2] = True
    ```

    Suppose we want to determine:

    ```text
    s[i ... j]
    ```

    For it to be a palindrome, first:

    ```python
    s[i] == s[j]
    ```

    must be true.

    Then we need to check whether the inside substring is also a palindrome:

    ```text
    s[i+1 ... j-1]
    ```

    which corresponds to:

    ```python
    dp[i + 1][j - 1]
    ```

    Therefore, for a normal substring:

    ```python
    dp[i][j] = (
        s[i] == s[j]
        and dp[i + 1][j - 1]
    )
    ```

    There is an important boundary case.

    ## Length = 1

    A single character is always a palindrome:

    ```text
    "a"
    "b"
    "c"
    ```

    ## Length = 2

    For example:

    ```text
    "aa"
    ```

    There is no inner substring that needs to be checked.

    As long as:

    ```text
    s[i] == s[j]
    ```

    the substring is a palindrome.

    So we use:

    ```python
    j - i <= 1
    ```

    to handle substrings of length `1` or `2`.

    Therefore, the complete transition is:

    ```python
    if s[i] == s[j] and (j - i <= 1 or dp[i + 1][j - 1]):
        dp[i][j] = True
    ```

    Equivalently:

    ```text
    dp[i][j] = True

    if:

    s[i] == s[j]

    AND

    (
        length <= 2
        OR
        dp[i+1][j-1] == True
    )
    ```

    This is one of the most important parts of this DP.

    Our transition is:

    ```python
    dp[i][j]
    ```

    depends on:

    ```python
    dp[i + 1][j - 1]
    ```

    So before calculating:

    ```text
    dp[i][j]
    ```

    we must already know:

    ```text
    dp[i+1][j-1]
    ```

    Therefore, `i` must iterate **from large to small**:

    ```python
    for i in range(n - 1, -1, -1):
    ```

    and `j` moves forward:

    ```python
    for j in range(i, n):
    ```

    Imagine the DP table:

    ```text
              j →
            0   1   2   3   4

    i   0   ×   ×   X   ×   ×
    ↓   1       ×   ↙   ×   ×
        2           ×   ×   ×
        3               ×   ×
        4                   ×
    ```

    To calculate:

    ```text
    dp[i][j]
    ```

    we need:

    ```text
    dp[i+1][j-1]
    ```

    which is located one row below and one column to the left.

    Therefore:

    ```text
    lower rows must be calculated first
    ```

    so `i` goes backward.

    We define:

    ```python
    dp[i][j]
    ```

    for substring:

    ```text
    s[i:j+1]
    ```

    A valid substring requires:

    ```text
    i <= j
    ```

    Therefore:

    ```python
    for j in range(i, n):
    ```

    We only calculate the upper-right half of the DP table.

    There is no need to calculate:

    ```text
    j < i
    ```

    because that does not represent a valid substring.

    Whenever:

    ```python
    dp[i][j] == True
    ```

    we have found a palindrome:

    ```text
    s[i:j+1]
    ```

    Its length is:

    ```python
    j - i + 1
    ```

    So if this palindrome is longer than the current maximum:

    ```python
    if j - i + 1 > maxLength:
    ```

    we update:

    ```python
    maxLength = j - i + 1
    start = i
    ```

    At the end, the longest palindrome starts at:

    ```text
    start
    ```

    and has length:

    ```text
    maxLength
    ```

    Therefore:

    ```python
    return s[start:start + maxLength]
    ```

    Consider:

    ```text
    s = "babad"
    ```

    Indices:

    ```text
    index:  0  1  2  3  4
            b  a  b  a  d
    ```

    ## Single Characters

    Every single character is a palindrome:

    ```text
    "b"
    "a"
    "b"
    "a"
    "d"
    ```

    Therefore:

    ```text
    dp[0][0] = True
    dp[1][1] = True
    dp[2][2] = True
    dp[3][3] = True
    dp[4][4] = True
    ```

    ## Check `"aba"`

    Consider:

    ```text
    i = 1
    j = 3
    ```

    Substring:

    ```text
    s[1:4] = "aba"
    ```

    First:

    ```text
    s[1] == s[3]

    'a' == 'a'
    ```

    Then check the inside:

    ```text
    dp[2][2]
    ```

    which represents:

    ```text
    "b"
    ```

    A single character is a palindrome:

    ```text
    dp[2][2] = True
    ```

    Therefore:

    ```text
    dp[1][3] = True
    ```

    So:

    ```text
    "aba"
    ```

    is a palindrome.

    ## Check `"bab"`

    Consider:

    ```text
    i = 0
    j = 2
    ```

    Substring:

    ```text
    "bab"
    ```

    We have:

    ```text
    s[0] == s[2]

    'b' == 'b'
    ```

    and:

    ```text
    dp[1][1] = True
    ```

    Therefore:

    ```text
    dp[0][2] = True
    ```

    So `"bab"` is also a palindrome.

    The longest length is:

    ```text
    3
    ```

    A common mistake is to write something like:

    ```python
    if s[i] == s[j]:
        dp[i][j] = dp[i - 1][j - 1] + 2
    else:
        dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    ```

    This type of transition allows us to **skip characters**.

    For example:

    ```python
    max(dp[i - 1][j], dp[i][j - 1])
    ```

    means:

    ```text
    ignore one character
    and keep the previous best result
    ```

    That is appropriate for a **subsequence** problem.

    But LeetCode 5 asks for:

    ```text
    Longest Palindromic Substring
    ```

    A substring must be continuous.

    Therefore, we cannot skip characters.

    This is the key difference between:

    ```text
    LeetCode 5
    Longest Palindromic Substring
    → continuous
    ```

    and:

    ```text
    LeetCode 516
    Longest Palindromic Subsequence
    → characters can be skipped
    ```

    ## Time Complexity

    There are approximately:

    ```text
    n × n
    ```

    DP states.

    Each state takes `O(1)` time.

    Therefore:

    ```text
    O(n²)
    ```

    ## Space Complexity

    We use an `n × n` DP table:

    ```text
    O(n²)
    ```



    The most important DP definition is:

    ```text
    dp[i][j]
    =
    whether s[i:j+1] is a palindrome
    ```

    The transition is:

    ```python
    dp[i][j] = (
        s[i] == s[j]
        and
        (j - i <= 1 or dp[i + 1][j - 1])
    )
    ```

    Remember the logic:

    ```text
    Two ends are equal
            +
    The inside is a palindrome
            ↓
    The whole substring is a palindrome
    ```

    And because:

    ```text
    dp[i][j]
    depends on
    dp[i+1][j-1]
    ```

    the iteration order must ensure the inner substring is calculated first:

    ```python
    for i in range(n - 1, -1, -1):
        for j in range(i, n):
    ```

    The main distinction to remember is:

    ```text
    Substring
    → continuous
    → cannot skip characters

    Subsequence
    → not necessarily continuous
    → can skip characters
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 14, 2026 |

---

[Back to Bonus Dynamic Programming](index.md) · [All Algorithm notes](../index.md)
