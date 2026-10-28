---
hide:
  - toc
---

# 118. Pascal’s Triangle

*Edited by Charles*

**Easy · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/pascals-triangle/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given an integer `numRows`, return the first `numRows` rows of Pascal's Triangle.

## Python solution

```python
class Solution:
    def generate(self, numRows: int) -> list[list[int]]:
        if numRows == 1:
            return [[1]]

        dp = [[] for _ in range(numRows)]
        dp[0] = [1]
        dp[1] = [1, 1]

        for i in range(2, numRows):
            dp[i] = (
                [1]
                + [
                    dp[i - 1][j] + dp[i - 1][j - 1]
                    for j in range(1, len(dp[i - 1]))
                ]
                + [1]
            )

        return dp
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given an integer `numRows`, return the first `numRows` rows of Pascal's Triangle.

    For example:

    ```text
    numRows = 5
    ```

    we should return:

    ```text
    [
        [1],
        [1, 1],
        [1, 2, 1],
        [1, 3, 3, 1],
        [1, 4, 6, 4, 1]
    ]
    ```

    Pascal's Triangle looks like:

    ```text
                1
              1   1
            1   2   1
          1   3   3   1
        1   4   6   4   1
    ```

    The key observation is:

    > Every row of Pascal's Triangle can be generated from the previous row.
    >

    We use:

    ```python
    dp[i]
    ```

    to represent the entire `i`-th row of Pascal's Triangle.

    Since Python uses 0-based indexing:

    ```text
    dp[0] = [1]
    dp[1] = [1, 1]
    dp[2] = [1, 2, 1]
    dp[3] = [1, 3, 3, 1]
    ...
    ```

    The first two rows are:

    ```text
    [1]

    [1, 1]
    ```

    Therefore:

    ```python
    dp = [[] for _ in range(numRows)]

    dp[0] = [1]
    dp[1] = [1, 1]
    ```

    Since accessing:

    ```python
    dp[1]
    ```

    requires at least two rows, we separately handle:

    ```python
    if numRows == 1:
        return [[1]]
    ```

    So the initialization is:

    ```python
    if numRows == 1:
        return [[1]]

    dp = [[] for _ in range(numRows)]

    dp[0] = [1]
    dp[1] = [1, 1]
    ```

    Every row of Pascal's Triangle has the following structure:

    ```text
    [1] + [middle elements] + [1]
    ```

    The first and last elements are always:

    ```text
    1
    ```

    For every middle element:

    ```text
    dp[i][j]
    ```

    we add the two adjacent elements from the previous row:

    ```python
    dp[i - 1][j - 1] + dp[i - 1][j]
    ```

    Therefore:

    ```text
    dp[i][j]
    =
    dp[i - 1][j - 1]
    +
    dp[i - 1][j]
    ```

    Consider:

    ```text
    Previous row:

            1     3     3     1
             \   / \   / \   /
              \ /   \ /   \ /
               4     6     4

    Current row:

            1     4     6     4     1
    ```

    For example:

    ```text
    4 = 1 + 3
    6 = 3 + 3
    4 = 3 + 1
    ```

    For position `j` in the current row, its two parents in the previous row are:

    ```text
    j - 1
    ```

    and:

    ```text
    j
    ```

    Therefore:

    ```python
    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
    ```

    We only need to calculate the **middle elements**.

    The first element is already:

    ```text
    1
    ```

    and the last element is also:

    ```text
    1
    ```

    Therefore, for the previous row:

    ```python
    dp[i - 1]
    ```

    we calculate:

    ```python
    for j in range(1, len(dp[i - 1]))
    ```

    For example:

    ```text
    previous row = [1, 3, 3, 1]
    ```

    Its length is:

    ```text
    4
    ```

    Therefore:

    ```python
    range(1, 4)
    ```

    gives:

    ```text
    j = 1, 2, 3
    ```

    and we calculate:

    ```text
    j = 1:

    dp[i-1][0] + dp[i-1][1]
    = 1 + 3
    = 4

    j = 2:

    dp[i-1][1] + dp[i-1][2]
    = 3 + 3
    = 6

    j = 3:

    dp[i-1][2] + dp[i-1][3]
    = 3 + 1
    = 4
    ```

    So the middle part becomes:

    ```text
    [4, 6, 4]
    ```

    Then we add `1` to both sides:

    ```text
    [1] + [4, 6, 4] + [1]
    ```

    giving:

    ```text
    [1, 4, 6, 4, 1]
    ```

    This allows us to construct one row directly using:

    ```python
    dp[i] = (
        [1]
        + [
            dp[i - 1][j] + dp[i - 1][j - 1]
            for j in range(1, len(dp[i - 1]))
        ]
        + [1]
    )
    ```

    This line directly represents the mathematical structure of Pascal's Triangle:

    ```text
    Current Row

    =
    [1]

    +

    [sum of every adjacent pair in the previous row]

    +

    [1]
    ```

    Consider:

    ```text
    numRows = 5
    ```

    Initially:

    ```text
    dp = [
        [1],
        [1, 1],
        [],
        [],
        []
    ]
    ```

    ## Generate Row 3

    Previous row:

    ```text
    dp[1] = [1, 1]
    ```

    Middle elements:

    ```text
    1 + 1 = 2
    ```

    Therefore:

    ```text
    [1] + [2] + [1]
    ```

    gives:

    ```text
    dp[2] = [1, 2, 1]
    ```

    Now:

    ```text
    dp = [
        [1],
        [1, 1],
        [1, 2, 1],
        [],
        []
    ]
    ```

    ## Generate Row 4

    Previous row:

    ```text
    dp[2] = [1, 2, 1]
    ```

    Middle elements:

    ```text
    1 + 2 = 3

    2 + 1 = 3
    ```

    Therefore:

    ```text
    [1] + [3, 3] + [1]
    ```

    gives:

    ```text
    dp[3] = [1, 3, 3, 1]
    ```

    ## Generate Row 5

    Previous row:

    ```text
    dp[3] = [1, 3, 3, 1]
    ```

    Middle elements:

    ```text
    1 + 3 = 4

    3 + 3 = 6

    3 + 1 = 4
    ```

    Therefore:

    ```text
    [1] + [4, 6, 4] + [1]
    ```

    gives:

    ```text
    dp[4] = [1, 4, 6, 4, 1]
    ```

    Final result:

    ```text
    [
        [1],
        [1, 1],
        [1, 2, 1],
        [1, 3, 3, 1],
        [1, 4, 6, 4, 1]
    ]
    ```

    Since we already initialize:

    ```python
    dp[1] = [1, 1]
    ```

    we could technically start from:

    ```python
    for i in range(2, numRows):
    ```

    instead of:

    ```python
    for i in range(1, numRows):
    ```

    because row `1` is already known.

    So a slightly cleaner version of the same approach is:

    ```python
    for i in range(2, numRows):
    ```

    This avoids calculating:

    ```text
    dp[1]
    ```

    again.

    ## Time Complexity

    We generate:

    ```text
    1 + 2 + 3 + ... + numRows
    ```

    elements.

    Therefore:

    ```text
    O(numRows²)
    ```

    This is already optimal because the output itself contains:

    ```text
    O(numRows²)
    ```

    numbers.

    ## Space Complexity

    The complete Pascal's Triangle contains:

    ```text
    O(numRows²)
    ```

    elements.

    Therefore:

    ```text
    O(numRows²)
    ```

    space is required for the returned result.



    Define:

    ```text
    dp[i] = the i-th row of Pascal's Triangle
    ```

    Every row has the structure:

    ```text
    [1] + middle + [1]
    ```

    For each middle position:

    ```python
    dp[i][j]
    =
    dp[i - 1][j - 1]
    +
    dp[i - 1][j]
    ```

    So the entire row can be constructed elegantly as:

    ```python
    dp[i] = (
        [1]
        + [
            dp[i - 1][j] + dp[i - 1][j - 1]
            for j in range(1, len(dp[i - 1]))
        ]
        + [1]
    )
    ```

    The most useful way to remember this problem is:

    ```text
    Pascal's Triangle

    current row
    =
    [1]
    +
    adjacent pair sums from previous row
    +
    [1]
    ```

    Time Complexity:

    ```text
    O(numRows²)
    ```

    Space Complexity:

    ```text
    O(numRows²)
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 18, 2026 |

---

[Back to Bonus Dynamic Programming](index.md) · [All Algorithm notes](../index.md)
