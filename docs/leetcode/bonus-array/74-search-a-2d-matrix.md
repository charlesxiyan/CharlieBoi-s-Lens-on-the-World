---
hide:
  - toc
---

# 74. Search a 2D Matrix

*Edited by Charles*

**Medium · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/search-a-2d-matrix/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    You are given an `m x n` integer matrix with the following properties:

    Each row is sorted in ascending order. The first integer of each row is greater than the last integer of the previous row.

## Python solution

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])

        left, right = 0, m * n - 1

        while left <= right:
            mid = (left + right) // 2

            row = mid // n
            col = mid % n

            if matrix[row][col] == target:
                return True

            elif matrix[row][col] < target:
                left = mid + 1

            else:
                right = mid - 1

        return False
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    You are given an `m x n` integer matrix with the following properties:

    1. Each row is sorted in ascending order.
    2. The first integer of each row is greater than the last integer of the previous row.

    Given an integer `target`, return `True` if `target` exists in the matrix. Otherwise, return `False`.

    ## Example

    ```text
    matrix =
    [
        [1,  3,  5,  7],
        [10, 11, 16, 20],
        [23, 30, 34, 60]
    ]

    target = 16
    ```

    Output:

    ```text
    True
    ```

    Because:

    ```text
    Each row is sorted
    +
    The first element of the next row
    is greater than the last element of the previous row
    ```

    the entire matrix can be considered as one sorted 1D array.

    For example:

    ```text
    [
        [1,  3,  5,  7],
        [10, 11, 16, 20],
        [23, 30, 34, 60]
    ]
    ```

    can logically be viewed as:

    ```text
    [1, 3, 5, 7, 10, 11, 16, 20, 23, 30, 34, 60]
    ```

    Therefore, we can directly apply Binary Search.

    Importantly, we do **not** need to actually flatten the matrix.

    ## Search Range

    Suppose:

    ```python
    m = len(matrix)
    n = len(matrix[0])
    ```

    There are:

    ```text
    m × n
    ```

    elements in total.

    Therefore, if we treat the matrix as a 1D array, its indices are:

    ```text
    0, 1, 2, ..., m × n - 1
    ```

    So:

    ```python
    left = 0
    right = m * n - 1
    ```

    ## Convert a 1D Index to a 2D Position

    This is the most important part of the problem.

    Suppose each row contains `n` elements.

    For a 1D index `mid`:

    ```python
    row = mid // n
    col = mid % n
    ```

    Therefore:

    ```python
    matrix[mid // n][mid % n]
    ```

    gives the element corresponding to index `mid` in the imaginary 1D array.

    ## Example

    Suppose:

    ```text
    matrix =
    [
        [1,  3,  5,  7],
        [10, 11, 16, 20],
        [23, 30, 34, 60]
    ]
    ```

    Here:

    ```text
    n = 4
    ```

    If:

    ```text
    mid = 6
    ```

    then:

    ```text
    row = 6 // 4 = 1
    col = 6 % 4  = 2
    ```

    Therefore:

    ```python
    matrix[1][2]
    ```

    is:

    ```text
    16
    ```

    This is exactly index `6` in the flattened array:

    ```text
    index:   0  1  2  3   4   5   6   7   8   9   10  11
             ↓  ↓  ↓  ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓

            [1, 3, 5, 7, 10, 11, 16, 20, 23, 30, 34, 60]
                                  ↑
                                mid = 6
    ```

    So the general mapping is:

    ```text
    1D index
        ↓
       mid
        ↓
    row = mid // n
    col = mid % n
        ↓
    matrix[row][col]
    ```

    ## Binary Search

    Once we know how to access the element at `mid`, the rest is exactly the same as standard Binary Search.

    Calculate:

    ```python
    mid = (left + right) // 2
    ```

    Then get the corresponding matrix value:

    ```python
    row = mid // n
    col = mid % n

    value = matrix[row][col]
    ```

    There are three cases.

    ### Case 1: Found the Target

    ```python
    if value == target:
        return True
    ```

    ### Case 2: Current Value Is Too Small

    If:

    ```python
    value < target
    ```

    then the target can only be on the right side.

    Therefore:

    ```python
    left = mid + 1
    ```

    ### Case 3: Current Value Is Too Large

    If:

    ```python
    value > target
    ```

    then the target can only be on the left side.

    Therefore:

    ```python
    right = mid - 1
    ```

    ## Overall Process

    ```text
    2D sorted matrix
           ↓
    Treat it as a 1D sorted array
           ↓
    left = 0
    right = m × n - 1
           ↓
    while left <= right
           ↓
    mid = (left + right) // 2
           ↓
    Convert mid to 2D coordinates
           ↓
    row = mid // n
    col = mid % n
           ↓
    value = matrix[row][col]
           ↓
     ┌─────────────┬─────────────┐
     ↓             ↓             ↓
    value          value          value
    == target      < target       > target
     ↓             ↓              ↓
    True       left=mid+1    right=mid-1
    ```

    ## Why Does This Work?

    The matrix satisfies:

    ```text
    matrix[i][0] > matrix[i - 1][n - 1]
    ```

    Therefore, after logically connecting all rows together, the resulting sequence is still sorted.

    For example:

    ```text
    Row 1:  1   3   5   7
                             ↓
    Row 2: 10  11  16  20
                             ↓
    Row 3: 23  30  34  60
    ```

    is equivalent to:

    ```text
    1 < 3 < 5 < 7 < 10 < 11 < 16 < 20 < 23 < 30 < 34 < 60
    ```

    Therefore, standard Binary Search can be applied to the entire matrix.

    ## Key Formula

    For a matrix with `n` columns:

    ```python
    row = index // n
    col = index % n
    ```

    This is a very useful mapping to remember:

    ```text
    1D index → 2D coordinates

    index // number_of_columns → row
    index %  number_of_columns → column
    ```

    ## Complexity

    There are:

    ```text
    m × n
    ```

    elements.

    Binary Search takes:

    ```text
    O(log(m × n))
    ```

    time.

    We do not create a flattened array, so the extra space complexity is:

    ```text
    O(1)
    ```

    ## Key Takeaway

    LeetCode 74 is essentially a standard Binary Search problem.

    The only special part is converting a virtual 1D index into a 2D matrix position:

    ```python
    matrix[mid // n][mid % n]
    ```

    So you can remember this problem as:

    ```text
    LC 74
    =
    Binary Search
    +
    1D → 2D index mapping
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 9, 2026 |

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
