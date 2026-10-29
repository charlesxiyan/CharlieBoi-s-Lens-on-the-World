---
hide:
  - toc
---

# 59. Spiral Matrix II

*Edited by Charles*

**Medium · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/spiral-matrix-ii/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        The main idea of this question to test the ability to imagine how the matrix is filled by the change of indices.

        Define 4 boundaries: top, bottom, left and right. The number to be filled starts at 1. Every time it is filled, it adds with 1.

    Original study notes follow, including intermediate snippets and earlier attempts.

    The main idea of this question to test the ability to imagine how the matrix is filled by the change of indices.

    1. Define 4 boundaries: top, bottom, left and right.
    2. The number to be filled starts at 1. Every time it is filled, it adds with 1.
    3. For the whole route, as long as top stays upper, bottom stays lower, and left does not surpass right:
        1. the upper: go through the indices (top, i) to fill the number; then update top to top + 1.
        2. the right: go through the indices (i, right) to fill the number; then update right to right - 1.
        3. the lower: go through the indices (bottom, i) to fill the number; then update bottom to bottom - 1.
        4. the left: go through the indices (i, left) to fill the number; then update left to left + 1.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 4 |
    | Mastery | ❚❚❚❚ |
    | The Latest Attempt | January 3, 2026 |

## Python solution

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        top, bottom, left, right = 0, n - 1, 0, n - 1
        matrix = [[0] * n for _ in range(n)]
        count = 1

        while top <= bottom and left <= right:
            for i in range(left, right + 1):
                matrix[top][i] = count
                count += 1
            top += 1

            for i in range(top, bottom + 1):
                matrix[i][right] = count
                count += 1
            right -= 1

            for i in range(right, left - 1, -1):
                matrix[bottom][i] = count
                count += 1
            bottom -= 1

            for i in range(bottom, top - 1, -1):
                matrix[i][left] = count
                count += 1
            left += 1

        return matrix
```

---

[Back to Day 2 Array II](index.md) · [All Algorithm notes](../index.md)
