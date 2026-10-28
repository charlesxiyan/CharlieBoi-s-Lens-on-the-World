---
hide:
  - toc
---

# 139. Word Break

*Edited by Charles*

**Medium · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/word-break/)

!!! tip "Core idea"

    - This is a **Complete Knapsack** problem.
    - The **string `s` is the knapsack**, and each **word is an item**.
    - Since a word can be used multiple times, it is a **Complete Knapsack**.
    - Goal: determine whether the entire string can be segmented into words from the dictionary.

## Python solution

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        l = len(s)

        dp = [False] * (l + 1)
        dp[0] = True

        for i in range(1, l + 1):
            for word in wordDict:
                if i >= len(word):
                    # 1. One true permutation has popped up.
                    # 2. The current suffix + dp[i - len(word)] permutation is either correct.
                    dp[i] = dp[i] or (dp[i - len(word)] and s[i - len(word): i] == word)

        return dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    - This is a **Complete Knapsack** problem.
    - The **string `s` is the knapsack**, and each **word is an item**.
    - Since a word can be used multiple times, it is a **Complete Knapsack**.
    - Goal: determine whether the entire string can be segmented into words from the dictionary.

    ## DP Definition

    dp[i] = whether the first `i` characters of `s` can be segmented into valid words.

    Initialize:
    dp = [False] * (len(s) + 1)
    dp[0] = True

    ## Why `dp[0] = True`?

    - `dp[0]` represents an empty string.
    - It is the starting point of all transitions.
    - Without it, no valid segmentation could ever begin.

    ## Iteration

    ### Outer loop

    for i in range(1, len(s) + 1):

    - `i` represents the current prefix length.
    - We check whether the first `i` characters can be segmented.

    ### Inner loop

    for word in wordDict:

    - Try every word as the **last word** of the current prefix.
    - Only consider words with `len(word) <= i`.

    ## Transition

    if dp[i - len(word)] and s[i - len(word):i] == word:
    dp[i] = True

    Meaning:

    - The prefix before the current word is already segmentable.
    - The current suffix exactly matches a dictionary word.
    - Therefore, the first `i` characters are segmentable.

    ## Why iterate `i` first?

    - The order of words matters.
    - Example:
    s = "applepenapple"
    wordDict = ["apple", "pen"]

    Only:
    apple + pen + apple

    is valid.

    Permutations like:
    pen + apple + apple

    do not match the string.

    Therefore, we iterate the **knapsack (prefix length)** first, then enumerate all possible words.

    ## Complexity

    Time: O(n × m × k)

    - n = length of `s`
    - m = number of words
    - k = average word length (substring comparison)

    Space: O(n)

    ## Template

    ```python
    for i in range(1, len(s) + 1):
        for word in wordDict:
            if i >= len(word):
                dp[i] |= dp[i - len(word)] and s[i - len(word):i] == word

    return dp[len(s)]
    ```

    Code:

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 9, 2026 |

---

[Back to Day 33 Dynamic Programming VI](index.md) · [All LeetCode notes](../index.md)
