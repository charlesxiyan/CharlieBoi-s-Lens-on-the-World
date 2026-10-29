---
hide:
  - toc
---

# 96. Unique Binary Search Trees

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/unique-binary-search-trees/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Quick Recap: n = 0, only one BST; n = 1, only one BST. n = 2, 2 BSTs. Explore n = 3:

        dp[i]: the number of unique BSTs.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Quick Recap: n = 0, only one BST; n = 1, only one BST. n = 2, 2 BSTs.
    2. Explore n = 3:
        1. root 1
            1. right node 2, right leaf 3
            2. right node 3, left leaf 2
        2. root 3
            1. left 2, left leaf 1
            2. left 1, right leaf 2
        3. root 2
            1. left leaf 1, right leaf 2
    3. DP Steps:
        1. **Step 1**: Define DP array states

            dp[i]: the number of unique BSTs.

            Note that the length of dp should be n + 1, ranging from 0 to n.

        2. **Step 2**: Recursive formula (n ≥ 1)

            dp[i] comes from dp[i - j] * dp[j - 1], i.e. the permutation of all possible cases of (i - j) nodes and all possible cases of (j - 1) cases, since there is still a root.

            e.g. dp[4], there are such scenarios:

            1. left subtree: 0 node, dp[0]; right subtree: 3 nodes, dp[3]
            2. left subtree: 1 node, dp[1]; right subtree: 2 nodes, dp[2]
            3. left subtree: 2 nodes, dp[2]; right subtree: 1 node, dp[1]
            4. left subtree: 3 nodes, dp[3]; right subtree: 0 node, dp[0]
        3. **Step 3**: Initial States

            dp[0] = 1

        4. **Step 4**: The order of Iteration:
            1. iteration i from number 1 to n.
            2. iteration j from 1 to i (range(1, i + 1))
        5. **Step 5**: Validation is omitted.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 2, 2026 |

## Python solution

```python
class Solution:
    def numTrees(self, n: int) -> int:
        if n <= 1:
            return 1

        dp = [0] * (n + 1)
        dp[0] = 1

        for i in range(1, n + 1):
            for j in range(1, i + 1):
                dp[i] += dp[i - j] * dp[j - 1]

        return dp[n]
```

---

[Back to Day 29 Dynamic Programming II](index.md) · [All Algorithm notes](../index.md)
