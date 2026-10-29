---
hide:
  - toc
---

# 583. Delete Operation for Two String

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/delete-operation-for-two-strings/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given two strings `word1` and `word2`, return the **minimum number of deletions** required to make the two strings equal.

        Unlike **LeetCode 115 (Distinct Subsequences)**, where only one string can delete characters, this problem allows **both strings** to delete characters.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given two strings `word1` and `word2`, return the **minimum number of deletions** required to make the two strings equal.

    Unlike **LeetCode 115 (Distinct Subsequences)**, where only one string can delete characters, this problem allows **both strings** to delete characters.

    For example:

    ```python
    word1 = "sea"
    word2 = "eat"
    ```

    One optimal solution is:

    ```text
    sea  -> ea   (delete 's')
    eat  -> ea   (delete 't')
    ```

    The minimum number of deletions is `2`.

    ## Dynamic Programming

    ### State Definition

    Let

    ```python
    dp[i][j]
    ```

    denote:

    > **The minimum number of deletions required to make `word1[:i]` and `word2[:j]` equal.**
    >

    In other words, considering the first `i` characters of `word1` and the first `j` characters of `word2`, `dp[i][j]` stores the minimum deletions needed to make the two prefixes identical.

    ### Initialization

    #### `dp[i][0]`

    If `word2` is empty, we must delete all characters from `word1`.

    Therefore,

    ```python
    dp[i][0] = i
    ```

    #### `dp[0][j]`

    Similarly, if `word1` is empty, we must delete all characters from `word2`.

    Therefore,

    ```python
    dp[0][j] = j
    ```

    ### State Transition

    There are two cases to consider.

    #### Case 1: Current characters match

    If

    ```python
    word1[i - 1] == word2[j - 1]
    ```

    the current characters do not need to be deleted.

    Simply inherit the previous result:

    ```python
    dp[i][j] = dp[i - 1][j - 1]
    ```

    #### Case 2: Current characters do not match

    If

    ```python
    word1[i - 1] != word2[j - 1]
    ```

    there are three possible operations.

    **Option 1: Delete the current character from `word1`**

    ```python
    dp[i - 1][j] + 1
    ```

    **Option 2: Delete the current character from `word2`**

    ```python
    dp[i][j - 1] + 1
    ```

    **Option 3: Delete both current characters**

    ```python
    dp[i - 1][j - 1] + 2
    ```

    Take the minimum of the three options:

    ```python
    dp[i][j] = min(
        dp[i - 1][j - 1] + 2,
        dp[i - 1][j] + 1,
        dp[i][j - 1] + 1
    )
    ```

    Transition equation:

    ```text
    if word1[i - 1] == word2[j - 1]:
        dp[i][j] = dp[i - 1][j - 1]
    else:
        dp[i][j] = min(
            dp[i - 1][j - 1] + 2,
            dp[i - 1][j] + 1,
            dp[i][j - 1] + 1
        )
    ```

    > **Note:**
    >
    >
    > Since
    >
    > ```
    > dp[i][j - 1] + 1
    > ```
    >
    > already represents deleting the current character of `word2`, deleting the current character of `word1` afterward is equivalent to deleting both current characters.
    >
    > Therefore, the transition can also be simplified to:
    >
    > ```python
    > dp[i][j] = min(dp[i - 1][j] + 1,
    >                dp[i][j - 1] + 1)
    > ```
    >
    > However, keeping all three cases makes the reasoning more intuitive.
    >

    ### Final Answer

    The answer is stored in

    ```python
    dp[len(word1)][len(word2)]
    ```

    which is equivalent to

    ```python
    dp[-1][-1]
    ```

    ## Example

    ```python
    word1 = "sea"
    word2 = "eat"
    ```

    Initialization:

    ```text
          ""  e  a  t
    ""     0  1  2  3
    s      1
    e      2
    a      3
    ```

    ### Compare 's' and 'e'

    ```text
    s != e
    ```

    Possible operations:

    - Delete `'s'`
    - Delete `'e'`
    - Delete both

    Take the minimum.

    ### Compare 'e' and 'e'

    ```text
    e == e
    ```

    Update:

    ```text
    dp[i][j] = dp[i-1][j-1]
    ```

    ### Compare 'a' and 'a'

    ```text
    a == a
    ```

    Again,

    ```text
    dp[i][j] = dp[i-1][j-1]
    ```

    Eventually,

    ```text
    dp[3][3] = 2
    ```

    meaning the minimum number of deletions is `2`.

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(m × n)
    ```

    where

    - `m = len(word1)`
    - `n = len(word2)`

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
    dp[i][j] = minimum deletions required to make word1[:i] and word2[:j] equal
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
            dp[i - 1][j - 1] + 2,
            dp[i - 1][j] + 1,
            dp[i][j - 1] + 1
        )
    ```

    **Traversal Order**

    Traverse both strings from left to right while filling the DP table.

    **Final Answer**

    ```python
    dp[-1][-1]
    ```

    ## Common Mistakes

    1. This problem only allows **deletion** operations.

        There is **no insertion** or **replacement**.

    2. When the current characters match,

    ```python
    dp[i][j] = dp[i - 1][j - 1]
    ```

    No deletion is needed.

    1. When the current characters do not match, consider deleting:
    - the current character from `word1`,
    - the current character from `word2`,
    - or both.
    1. The initialization is important.

    ```python
    dp[i][0] = i
    ```

    means deleting all characters from `word1`.

    ```python
    dp[0][j] = j
    ```

    means deleting all characters from `word2`.

    1. This problem is closely related to **LeetCode 72 (Edit Distance)**, but is simpler because **only deletion operations are allowed**.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

## Python solution

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        l1 = len(word1)
        l2 = len(word2)

        dp = [[0] * (1 + l2) for _ in range(1 + l1)]

        for i in range(l1 + 1):
            dp[i][0] = i
        for j in range(l2 + 1):
            dp[0][j] = j

        for i in range(1, l1 + 1):
            for j in range(1, l2 + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = min(dp[i - 1][j - 1] + 2, dp[i - 1][j] + 1, dp[i][j - 1] + 1)

        return dp[-1][-1]
```

---

[Back to Day 39 Dynamic Programming XII](index.md) · [All Algorithm notes](../index.md)
