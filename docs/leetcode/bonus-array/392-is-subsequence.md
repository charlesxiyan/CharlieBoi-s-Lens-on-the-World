---
hide:
  - toc
---

# 392. Is Subsequence

*Edited by Charles*

**Easy · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/is-subsequence/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Use two pointers:

        - `i` points to the current character in `s`
        - `j` points to the current character in `t`

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given two strings `s` and `t`, return `True` if `s` is a subsequence of `t`, otherwise return `False`.

    A subsequence is a string that can be derived from another string by deleting some characters without changing the relative order of the remaining characters.

    ### Example

    ```text
    Input:
    s = "abc"
    t = "ahbgdc"

    Output:
    True
    ```

    ```text
    Input:
    s = "axc"
    t = "ahbgdc"

    Output:
    False
    ```

    ## Approach: Two Pointers

    Use two pointers:

    - `i` points to the current character in `s`
    - `j` points to the current character in `t`

    ```text
    s: a b c
       ↑
       i

    t: a h b g d c
       ↑
       j
    ```

    The pointer `j` always moves forward because we are searching through `t`.

    The pointer `i` only moves forward when we successfully find `s[i]` in `t`.

    ### Case 1: Characters Match

    If:

    ```python
    s[i] == t[j]
    ```

    we have found the current character of `s`, so move `i` forward:

    ```python
    i += 1
    ```

    ### Case 2: Characters Do Not Match

    If:

    ```python
    s[i] != t[j]
    ```

    we still need to find `s[i]`, so `i` stays unchanged.

    We only move `j` forward to continue searching in `t`.

    Therefore, `j` moves forward in every iteration:

    ```python
    j += 1
    ```

    ## Example Walkthrough

    ```text
    s = "abc"
    t = "ahbgdc"
    ```

    Start:

    ```text
    s: a b c
       ↑
       i

    t: a h b g d c
       ↑
       j
    ```

    ### Step 1

    ```text
    a == a
    ```

    Match:

    ```text
    i → b
    j → h
    ```

    ### Step 2

    ```text
    b != h
    ```

    Keep `i` at `b`, move `j`:

    ```text
    i → b
    j → b
    ```

    ### Step 3

    ```text
    b == b
    ```

    Match:

    ```text
    i → c
    j → g
    ```

    ### Step 4

    ```text
    c != g
    ```

    Move `j`.

    ### Step 5

    ```text
    c != d
    ```

    Move `j`.

    ### Step 6

    ```text
    c == c
    ```

    Now all characters in `s` have been matched.

    Therefore:

    ```python
    i == len(s)
    ```

    and the answer is `True`.

    ## Why `return i == len(s)`?

    `i` represents how many characters of `s` have been successfully matched.

    If:

    ```python
    i == len(s)
    ```

    then every character in `s` has been found in `t` in the correct order.

    Therefore, `s` is a subsequence of `t`.

    ## Complexity

    Let `n = len(t)`.

    - **Time:** `O(n)`
    - **Space:** `O(1)`

    Each character in `t` is visited at most once.

    ## Key Idea

    ```text
    Pointer in t:
    always moves forward

    Pointer in s:
    moves forward only when a match is found
    ```

    In code:

    ```python
    if s[i] == t[j]:
        i += 1

    j += 1
    ```

    This preserves the relative order of characters in `s`, which is exactly what the definition of a subsequence requires.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | October 3, 2026 |

## Python solution

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i, j = 0, 0

        while i < len(s) and j < len(t):
            if s[i] == t[j]:
                i += 1

            j += 1

        return i == len(s)
```

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
