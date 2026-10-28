---
hide:
  - toc
---

# 37. Sudoku Solver

*Edited by Charles*

**Hard · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/sudoku-solver/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    We use **backtracking** to fill the empty cells one by one.

    For each empty cell, we try digits `'1'` through `'9'`. A digit can be placed only if it does not already exist in the corresponding row, column, or `3 × 3` sub-box.

## Python solution

```python
from collections import defaultdict

class Solution:
    def solveSudoku(self, board: list[list[str]]) -> None:
        rows = defaultdict(set)
        cols = defaultdict(set)
        boxes = defaultdict(set)
        empty = []

        # Initialize
        for i in range(9):
            for j in range(9):
                num = board[i][j]

                if num == '.':
                    empty.append((i, j))
                    continue

                block = (i // 3, j // 3)

                rows[i].add(num)
                cols[j].add(num)
                boxes[block].add(num)

        def backtrack(index):
            # Base case: all empty cells are filled
            if index == len(empty):
                return True

            i, j = empty[index]
            block = (i // 3, j // 3)

            for num in '123456789':
                # Check validity
                if num in rows[i] or num in cols[j] or num in boxes[block]:
                    continue

                # Make choice
                board[i][j] = num
                rows[i].add(num)
                cols[j].add(num)
                boxes[block].add(num)

                # Explore
                if backtrack(index + 1):
                    return True

                # Undo choice
                board[i][j] = '.'
                rows[i].remove(num)
                cols[j].remove(num)
                boxes[block].remove(num)

            return False

        backtrack(0)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem Description

    Given a partially filled `9 × 9` Sudoku board, solve the puzzle by filling the empty cells.

    A valid Sudoku solution must satisfy three rules:

    1. Each row contains the digits `1–9` exactly once.
    2. Each column contains the digits `1–9` exactly once.
    3. Each `3 × 3` sub-box contains the digits `1–9` exactly once.

    Empty cells are represented by `'.'`.

    We must modify the board **in-place** without returning anything.

    ## Approach — Backtracking + Hash Table

    We use **backtracking** to fill the empty cells one by one.

    For each empty cell, we try digits `'1'` through `'9'`. A digit can be placed only if it does not already exist in the corresponding row, column, or `3 × 3` sub-box.

    To efficiently check whether a digit is valid, we maintain three hash tables:

    ```python
    rows = defaultdict(set)
    cols = defaultdict(set)
    boxes = defaultdict(set)
    ```

    - `rows[i]`: Digits already used in row `i`.
    - `cols[j]`: Digits already used in column `j`.
    - `boxes[(i // 3, j // 3)]`: Digits already used in the corresponding `3 × 3` sub-box.

    We also store all empty cells in a list:

    ```python
    empty = [(i, j), ...]
    ```

    This avoids scanning the entire board during every recursive call.

    ### Identifying the 3 × 3 Sub-box

    For a cell at position `(i, j)`, its sub-box is identified by:

    ```python
    block = (i // 3, j // 3)
    ```

    The nine sub-boxes are indexed as:

    ```text
    (0,0) | (0,1) | (0,2)
    ------+-------+------
    (1,0) | (1,1) | (1,2)
    ------+-------+------
    (2,0) | (2,1) | (2,2)
    ```

    For example, cell `(4, 5)` belongs to sub-box `(1, 1)`.

    ## Backtracking Algorithm

    We define:

    ```python
    backtrack(index)
    ```

    where `index` represents the position of the current empty cell in the `empty` list.

    ### Step 1 — Base Case

    ```python
    if index == len(empty):
        return True
    ```

    If all empty cells have been filled successfully, the Sudoku is solved.

    ### Step 2 — Select the Current Empty Cell

    ```python
    i, j = empty[index]
    block = (i // 3, j // 3)
    ```

    ### Step 3 — Try Digits 1 Through 9

    For each candidate digit:

    ```python
    for num in '123456789':
    ```

    Check whether it already exists in the current row, column, or sub-box:

    ```python
    if num in rows[i] or num in cols[j] or num in boxes[block]:
        continue
    ```

    If the digit is already used, skip it and try the next candidate.

    Otherwise, place the digit and update the hash tables.

    ### Step 4 — Recursively Solve the Next Cell

    ```python
    if backtrack(index + 1):
        return True
    ```

    If the recursive call returns `True`, the remaining cells have been filled successfully. We propagate `True` upward to preserve the solution.

    ### Step 5 — Undo the Choice

    If the recursive call returns `False`, the current choice cannot lead to a valid solution.

    We must restore the previous state:

    ```python
    board[i][j] = '.'
    rows[i].remove(num)
    cols[j].remove(num)
    boxes[block].remove(num)
    ```

    Then we try another digit.

    If none of the nine digits leads to a solution:

    ```python
    return False
    ```

    This signals the previous recursive call to backtrack.

    ## Example — Hand Simulation

    Suppose the current board contains an empty cell at `(0, 2)`.

    Assume the existing digits are:

    ```python
    rows[0] = {'5', '3', '7'}
    cols[2] = {'8'}
    boxes[(0, 0)] = {'5', '3', '6', '9', '8'}
    ```

    The available candidates are:

    ```text
    1, 2, 4
    ```

    We first try `'1'`.

    **Make the choice:**

    ```python
    board[0][2] = '1'
    rows[0].add('1')
    cols[2].add('1')
    boxes[(0, 0)].add('1')
    ```

    **Explore the next empty cell:**

    ```python
    backtrack(index + 1)
    ```

    There are two possibilities:

    **Case A — The recursive call returns `True`**

    All remaining empty cells can be filled successfully.

    We return `True` immediately, keeping the current digit.

    **Case B — The recursive call returns `False`**

    The choice `'1'` eventually leads to a dead end.

    We undo the choice:

    ```python
    board[0][2] = '.'
    rows[0].remove('1')
    cols[2].remove('1')
    boxes[(0, 0)].remove('1')
    ```

    Then we try `'2'`, followed by `'4'` if necessary.

    This process continues until we find a complete solution.

    ## Why Do We Need Both `return True` and `return False`?

    | Return value | Meaning |
    |---|---|
    | `True` | All empty cells have been successfully filled. |
    | `False` | No candidate works for the current cell under the previous choices. |

    The key distinction is:

    - `continue` means **this particular digit is invalid**.
    - `return False` means **all candidate digits have failed**.
    - `return True` means **a complete solution has been found**.

    ## Complexity Analysis

    Let `m` be the number of empty cells.

    **Time Complexity: O(9^m)** in the worst case.

    Each empty cell may have up to nine candidate digits. Although Sudoku constraints eliminate many choices, the theoretical worst-case search space is exponential.

    Checking whether a digit is already used takes O(1) on average because we use hash-based sets.

    **Space Complexity: O(m)** for the recursion stack and the list of empty cells, plus O(1) auxiliary storage for the row, column, and sub-box sets because the Sudoku board size is fixed.

    ## Key Takeaways

    - **Backtracking** explores possible assignments and undoes choices when a dead end is reached.
    - **Hash-based sets** allow efficient validation of rows, columns, and sub-boxes.
    - **Precomputing empty cells** avoids repeatedly scanning the entire board.
    - **`return True`** stops the entire search as soon as a complete solution is found.
    - **`return False`** triggers backtracking when no valid candidate remains.
    - The board is modified **in-place**, and successful assignments are preserved.

    **Related Problems:**

    - LeetCode 36 — Valid Sudoku
    - LeetCode 51 — N-Queens
    - LeetCode 39 — Combination Sum
    - LeetCode 79 — Word Search

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | October 24, 2026 |

---

[Back to Day 22 Backtrack IV](index.md) · [All Algorithm notes](../index.md)
