---
hide:
  - toc
---

# 115. Distinct Subsequence

*Edited by Charles*

**Hard · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/distinct-subsequences/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given two strings `s` and `t`, return the number of distinct subsequences of `s` that equal `t`.

    Unlike **LeetCode 392 (Is Subsequence)**, which only asks whether `t` is a subsequence of `s`, this problem asks for **how many different ways** `t` can be formed from `s`.

## Python solution

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        l1 = len(s)
        l2 = len(t)

        dp = [[0] * (l2 + 1) for _ in range(l1 + 1)]

        for i in range(l1 + 1):
            dp[i][0] = 1

        for i in range(1, l1 + 1):
            for j in range(1, l2 + 1):
                if s[i - 1] == t[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
                else:
                    dp[i][j] = dp[i - 1][j]

        return dp[-1][-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given two strings `s` and `t`, return the number of distinct subsequences of `s` that equal `t`.

    Unlike **LeetCode 392 (Is Subsequence)**, which only asks whether `t` is a subsequence of `s`, this problem asks for **how many different ways** `t` can be formed from `s`.

    A subsequence is obtained by deleting some (or no) characters without changing the relative order of the remaining characters.

    For example:

    ```python
    s = "rabbbit"
    t = "rabbit"
    ```

    There are **3** different ways to delete one `'b'` from `s` to obtain `"rabbit"`.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The number of distinct subsequences of `s[:i]` that equal `t[:j]`.**
    >

    In other words, we consider the first `i` characters of `s` and count how many different ways they can form the first `j` characters of `t`.

    ### Initialization

    #### `dp[i][0]`

    An empty string `t` can always be formed by deleting **all** characters from `s`.

    Therefore,

    ```python
    dp[i][0] = 1
    ```

    for every `i`.

    #### `dp[0][j]`

    A non-empty string cannot be formed from an empty string.

    Therefore,

    ```python
    dp[0][j] = 0
    ```

    for every `j > 0`.

    #### `dp[0][0]`

    Both strings are empty.

    There is exactly one way to transform an empty string into another empty string.

    ```python
    dp[0][0] = 1
    ```

    ### State Transition

    There are two cases to consider.

    #### Case 1: Current characters match

    If

    ```python
    s[i - 1] == t[j - 1]
    ```

    there are **two possible choices**.

    **Choice 1: Use `s[i-1]` to match `t[j-1]`.**

    Then the remaining prefixes must also match.

    The number of ways is

    ```python
    dp[i - 1][j - 1]
    ```

    **Choice 2: Skip `s[i-1]`.**

    Even though the characters are equal, we may choose **not** to use the current character in `s`.

    The number of ways is

    ```python
    dp[i - 1][j]
    ```

    For example,

    ```text
    s = "bagg"
    t = "bag"
    ```

    The last `'g'` in `s` can be used to match `'g'` in `t`:

    ```text
    bag(g)
    ```

    or it can be ignored:

    ```text
    (bag)g
    ```

    Both are valid subsequences.

    Therefore,

    ```python
    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
    ```

    #### Case 2: Current characters do not match

    If

    ```python
    s[i - 1] != t[j - 1]
    ```

    the current character in `s` cannot contribute to matching `t`.

    The only option is to skip it.

    ```python
    dp[i][j] = dp[i - 1][j]
    ```

    Notice that we **never skip characters in `t`**.

    This is because the problem asks:

    > **How many subsequences of `s` equal `t`?**
    >

    We are deleting characters from `s`, not from `t`.

    ### Transition Equation

    ```text
    if s[i - 1] == t[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
    else:
        dp[i][j] = dp[i - 1][j]
    ```

    ### Final Answer

    The answer is stored in

    ```python
    dp[len(s)][len(t)]
    ```

    which is equivalent to

    ```python
    dp[-1][-1]
    ```

    ## Example

    ```python
    s = "babgbag"
    t = "bag"
    ```

    Initialization:

    ```text
          ""  b  a  g
    ""     1  0  0  0
    b      1
    a      1
    b      1
    g      1
    b      1
    a      1
    g      1
    ```

    Whenever the current characters match, we add two possibilities:

    ```text
    Use the current character
            +
    Skip the current character
    ```

    Eventually,

    ```text
    dp[7][3] = 5
    ```

    meaning there are **5 distinct subsequences** of `"babgbag"` equal to `"bag"`.

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

    > **Note:** Since each state only depends on the previous row, the DP table can be optimized to **O(n)** using a rolling array.
    >

    ## Summary

    **State Definition**

    ```text
    dp[i][j] = number of distinct subsequences of s[:i] that equal t[:j]
    ```

    **Initialization**

    ```python
    dp[i][0] = 1
    dp[0][j] = 0
    dp[0][0] = 1
    ```

    **State Transition**

    ```python
    if s[i - 1] == t[j - 1]:
        dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
    else:
        dp[i][j] = dp[i - 1][j]
    ```

    **Traversal Order**

    Traverse both strings from left to right while filling the DP table.

    **Final Answer**

    ```python
    dp[-1][-1]
    ```

    ## Common Mistakes

    1. Do **not** confuse this problem with **LeetCode 392 (Is Subsequence)**.
        - **392** asks whether `t` is a subsequence of `s`.
        - **115** asks **how many different subsequences** of `s` equal `t`.
    2. When the current characters match, remember there are **two choices**:

    ```text
    Use the current character.
    Skip the current character.
    ```

    Therefore,

    ```python
    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
    ```

    1. When the characters do not match,

    ```python
    dp[i][j] = dp[i - 1][j]
    ```

    Only skip characters in `s`, **never** in `t`.

    1. The initialization is easy to get wrong.

    ```python
    dp[i][0] = 1
    ```

    because every prefix of `s` contains exactly one empty subsequence.

    ```python
    dp[0][j] = 0
    ```

    because an empty string can never form a non-empty target string.

    1. This is a **counting DP** problem rather than an optimization problem, so the state stores **the number of ways**, not the maximum or minimum length.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 39 Dynamic Programming XII](index.md) · [All Algorithm notes](../index.md)
