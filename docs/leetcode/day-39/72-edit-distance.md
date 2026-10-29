---
hide:
  - toc
---

# 72. Edit Distance

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/edit-distance/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Use `dp[i][j]` for the edit distance between the first `i` and `j` characters. Matching characters reuse the diagonal; otherwise choose insertion, deletion, or replacement.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` into `word2`.

    The allowed operations are:

    - Insert a character
    - Delete a character
    - Replace a character

    Edit Distance is a classic dynamic programming problem. Although the problem looks complicated because there are multiple possible operations, the DP transition is very structured.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The minimum edit distance between `word1[:i]` and `word2[:j]`.**
    >

    That means:

    - `word1[:i]` contains the first `i` characters of `word1`
    - `word2[:j]` contains the first `j` characters of `word2`

    So `dp[i][j]` stores the minimum number of operations needed to transform the first `i` characters of `word1` into the first `j` characters of `word2`.

    ### Initialization

    #### `dp[i][0]`

    If `word2` is empty, we must delete every character from `word1[:i]`.

    Therefore,

    ```python
    dp[i][0] = i
    ```

    For example:

    ```text
    "abc" -> ""
    ```

    requires 3 deletions.

    #### `dp[0][j]`

    If `word1` is empty, we must insert every character from `word2[:j]`.

    Therefore,

    ```python
    dp[0][j] = j
    ```

    For example:

    ```text
    "" -> "abc"
    ```

    requires 3 insertions.

    ### State Transition

    There are two main cases.

    #### Case 1: Current characters are equal

    If

    ```python
    word1[i - 1] == word2[j - 1]
    ```

    the current characters already match, so no additional operation is needed.

    Therefore,

    ```python
    dp[i][j] = dp[i - 1][j - 1]
    ```

    For example:

    ```text
    word1 = "ab"
    word2 = "cb"
    ```

    Since the last characters are both `'b'`, the problem reduces to transforming:

    ```text
    "a" -> "c"
    ```

    #### Case 2: Current characters are different

    If

    ```python
    word1[i - 1] != word2[j - 1]
    ```

    we have three possible operations.

    ### Option 1: Delete from `word1`

    Delete `word1[i - 1]`.

    Then we need to transform:

    ```text
    word1[:i-1] -> word2[:j]
    ```

    So the cost is:

    ```python
    dp[i - 1][j] + 1
    ```

    ### Option 2: Insert into `word1`

    Insert `word2[j - 1]` into `word1`.

    After the insertion, `word2[j - 1]` is matched, so we still need to transform:

    ```text
    word1[:i] -> word2[:j-1]
    ```

    The cost is:

    ```python
    dp[i][j - 1] + 1
    ```

    Another way to understand this is:

    > Inserting a character into `word1` is equivalent, from the DP perspective, to deleting the corresponding character from `word2`.
    >

    ### Option 3: Replace

    Replace `word1[i - 1]` with `word2[j - 1]`.

    After one replacement, the last characters match, so the remaining problem is:

    ```text
    word1[:i-1] -> word2[:j-1]
    ```

    The cost is:

    ```python
    dp[i - 1][j - 1] + 1
    ```

    ### Transition Equation

    If the current characters match:

    ```python
    dp[i][j] = dp[i - 1][j - 1]
    ```

    Otherwise:

    ```python
    dp[i][j] = min(
        dp[i - 1][j - 1] + 1,  # replace
        dp[i - 1][j] + 1,      # delete
        dp[i][j - 1] + 1       # insert
    )
    ```

    Equivalent form:

    ```python
    dp[i][j] = min(
        dp[i - 1][j - 1],
        dp[i - 1][j],
        dp[i][j - 1]
    ) + 1
    ```

    when the current characters are different.

    ### Traversal Order

    Each state depends on:

    ```text
    dp[i - 1][j - 1]   # upper-left
    dp[i - 1][j]       # above
    dp[i][j - 1]       # left
    ```

    Therefore, the DP table must be filled:

    ```text
    Top to bottom
    Left to right
    ```

    so that all required states have already been computed.

    ### Final Answer

    The final answer is:

    ```python
    dp[len(word1)][len(word2)]
    ```

    which is equivalent to:

    ```python
    dp[-1][-1]
    ```

    ## Example

    ```python
    word1 = "horse"
    word2 = "ros"
    ```

    One optimal sequence of operations is:

    ```text
    horse
    ↓ replace 'h' with 'r'
    rorse
    ↓ delete 'r'
    rose
    ↓ delete 'e'
    ros
    ```

    Total operations:

    ```text
    3
    ```

    So the answer is:

    ```python
    3
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    where:

    - `m = len(word1)`
    - `n = len(word2)`

    Each DP state is computed once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    A 2D DP table is used.

    > **Note:** The space complexity can be optimized to `O(n)` using a rolling array.
    >

    ## Summary

    **State Definition**

    ```text
    dp[i][j] = minimum number of operations required to convert word1[:i] into word2[:j]
    ```

    **Initialization**

    ```python
    dp[i][0] = i
    dp[0][j] = j
    ```

    **State Transition**

    ```python
    if word1[i - 1] == word2[j - 1]:
        dp[i][j] = dp[i - 1][j - 1]
    else:
        dp[i][j] = min(
            dp[i - 1][j - 1] + 1,
            dp[i - 1][j] + 1,
            dp[i][j - 1] + 1
        )
    ```

    **Operation Mapping**

    ```text
    dp[i - 1][j - 1] + 1  -> Replace
    dp[i - 1][j] + 1      -> Delete from word1
    dp[i][j - 1] + 1      -> Insert into word1
    ```

    **Traversal Order**

    ```text
    Top to bottom, left to right
    ```

    **Final Answer**

    ```python
    dp[-1][-1]
    ```

    ## Common Mistakes

    1. Do not forget that this problem allows **three operations**:

    ```text
    Insert
    Delete
    Replace
    ```

    1. When the current characters are equal:

    ```python
    word1[i - 1] == word2[j - 1]
    ```

    no additional operation is needed:

    ```python
    dp[i][j] = dp[i - 1][j - 1]
    ```

    1. Be careful with the meaning of the three neighboring states:

    ```text
    dp[i - 1][j]     -> delete from word1
    dp[i][j - 1]     -> insert into word1
    dp[i - 1][j - 1] -> replace
    ```

    1. The initialization must reflect conversions involving an empty string:

    ```python
    dp[i][0] = i
    dp[0][j] = j
    ```

    1. Compared with **LeetCode 583**, this problem allows replacement in addition to insertion and deletion, so:

    ```text
    LeetCode 583 -> deletion only
    LeetCode 72  -> insertion + deletion + replacement
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 11, 2026 |

## Python solution

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        l1 = len(word1)
        l2 = len(word2)

        dp = [[0] * (l2 + 1) for _ in range(l1 + 1)]
        for i in range(l1 + 1):
            dp[i][0] = i
        for j in range(l2 + 1):
            dp[0][j] = j

        for i in range(1, l1 + 1):
            for j in range(1, l2 + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = min(dp[i - 1][j - 1] + 1, dp[i - 1][j] + 1, dp[i][j - 1] + 1)

        return dp[-1][-1]
```

---

[Back to Day 39 Dynamic Programming XII](index.md) · [All Algorithm notes](../index.md)
