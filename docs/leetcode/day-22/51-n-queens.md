---
hide:
  - toc
---

# 51. N-Queens

*Edited by Charles*

**Hard · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/n-queens/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        See the Python implementation and detailed notes below.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: Backtracking

    ### Core Idea

    We need to place `n` queens on an `n × n` chessboard such that no two queens attack each other.

    Two queens attack each other if they are in the same:

    - row
    - column
    - diagonal

    The key idea is:

    > Place exactly one queen in each row.
    >

    Therefore, each level of recursion represents one row.

    For the current row, we try every column:

    ```python
    for j in range(n):
    ```

    If placing a queen at:

    ```text
    (row, j)
    ```

    is valid, we:

    1. place the queen
    2. recursively move to the next row
    3. remove the queen when backtracking

    ## Board Representation

    We initialize the board as:

    ```python
    [['.'] * n for _ in range(n)]
    ```

    For example, when `n = 4`:

    ```text
    . . . .
    . . . .
    . . . .
    . . . .
    ```

    A queen is represented by:

    ```text
    Q
    ```

    and an empty position is represented by:

    ```text
    .
    ```

    ## Meaning of `row`

    The recursion starts with:

    ```python
    self.backtrack(0, n, board)
    ```

    Here:

    ```text
    row = current row where we want to place a queen
    ```

    Each recursive call moves to the next row:

    ```python
    self.backtrack(row + 1, n, board)
    ```

    So the recursion structure is:

    ```text
    row 0
      ↓
    row 1
      ↓
    row 2
      ↓
    ...
      ↓
    row n
    ```

    This automatically guarantees that there is at most one queen in each row.

    Therefore, we do not need to check whether another queen exists in the same row.

    ## Try Every Column

    For the current row:

    ```python
    for j in range(n):
    ```

    we try placing the queen in every possible column:

    ```text
    (row, 0)
    (row, 1)
    (row, 2)
    ...
    (row, n - 1)
    ```

    Before placing the queen, we check:

    ```python
    if self.isValid(row, j, board, n):
    ```

    If the position is valid:

    ```python
    board[row][j] = 'Q'
    ```

    then recursively process the next row:

    ```python
    self.backtrack(row + 1, n, board)
    ```

    After returning from recursion, undo the choice:

    ```python
    board[row][j] = '.'
    ```

    This is the standard backtracking pattern:

    ```text
    make a choice
        ↓
    recurse
        ↓
    undo the choice
    ```

    ## Base Case

    When:

    ```python
    row == n
    ```

    it means we have successfully placed one queen in every row.

    Therefore, we found one valid solution:

    ```python
    if row == n:
        self.res.append([''.join(r) for r in board])
        return
    ```

    The board currently looks like:

    ```python
    [
        ['.', 'Q', '.', '.'],
        ['.', '.', '.', 'Q'],
        ['Q', '.', '.', '.'],
        ['.', '.', 'Q', '.']
    ]
    ```

    But LeetCode requires:

    ```python
    [
        ".Q..",
        "...Q",
        "Q...",
        "..Q."
    ]
    ```

    Therefore, we convert every row into a string:

    ```python
    [''.join(r) for r in board]
    ```

    Since we place queens row by row from top to bottom, when we are trying to place a queen at:

    ```text
    (row, col)
    ```

    all existing queens must be above the current row.

    Therefore, we only need to check:

    ```text
    1. Same column
    2. Upper-left diagonal
    3. Upper-right diagonal
    ```

    We do not need to check:

    ```text
    same row
    lower-left diagonal
    lower-right diagonal
    ```

    because:

    - the current row does not contain another queen
    - lower rows have not been processed yet

    ## Check the Same Column

    We check whether there is already a queen in the same column:

    ```python
    for i in range(n):
        if board[i][col] == 'Q':
            return False
    ```

    For example:

    ```text
    . . Q .
    . . . .
    . . X .
    . . . .
    ```

    If we want to put a queen at `X`, there is already a queen above it in the same column.

    Therefore, the position is invalid.

    Conceptually, because queens are placed from top to bottom, checking only:

    ```python
    for i in range(row):
    ```

    would also be enough.

    But the current code checks the whole column, which is also correct.

    ## Check the Upper-Left Diagonal

    Starting from:

    ```text
    (row - 1, col - 1)
    ```

    we continuously move:

    ```text
    ↖
    ```

    by doing:

    ```python
    rowIdx -= 1
    colIdx -= 1
    ```

    Code:

    ```python
    rowIdx, colIdx = row - 1, col - 1

    while rowIdx >= 0 and colIdx >= 0:
        if board[rowIdx][colIdx] == 'Q':
            return False

        rowIdx -= 1
        colIdx -= 1
    ```

    Example:

    ```text
    Q . . .
    . X . .
    . . . .
    . . . .
    ```

    The queen and `X` are on the same diagonal, so `X` is invalid.

    ## Check the Upper-Right Diagonal

    Starting from:

    ```text
    (row - 1, col + 1)
    ```

    we continuously move:

    ```text
    ↗
    ```

    by doing:

    ```python
    rowIdx -= 1
    colIdx += 1
    ```

    Code:

    ```python
    rowIdx, colIdx = row - 1, col + 1

    while rowIdx >= 0 and colIdx < n:
        if board[rowIdx][colIdx] == 'Q':
            return False

        rowIdx -= 1
        colIdx += 1
    ```

    Example:

    ```text
    . . . Q
    . . X .
    . . . .
    . . . .
    ```

    Again, `X` is attacked diagonally by the queen.

    Consider:

    ```text
    n = 4
    ```

    Initially:

    ```text
    ....
    ....
    ....
    ....
    ```

    Start with:

    ```text
    row = 0
    ```

    Try column `0`:

    ```text
    Q...
    ....
    ....
    ....
    ```

    Then recursively process:

    ```text
    row = 1
    ```

    Column `0` is invalid because of the same column:

    ```text
    Q...
    X...
    ....
    ....
    ```

    Column `1` is invalid because of the diagonal:

    ```text
    Q...
    .X..
    ....
    ....
    ```

    Column `2` is valid:

    ```text
    Q...
    ..Q.
    ....
    ....
    ```

    Then recursively process row `2`.

    If no valid position can be found later, we backtrack:

    ```python
    board[row][j] = '.'
    ```

    and try the next column.

    Therefore, the search explores possibilities like:

    ```text
    row 0
    ├── col 0
    │   ├── row 1 col 2
    │   │   └── ...
    │   └── row 1 col 3
    │       └── ...
    │
    ├── col 1
    │   └── ...
    │
    ├── col 2
    │   └── ...
    │
    └── col 3
        └── ...
    ```

    Invalid branches are removed immediately by `isValid()`.

    This is the pruning step of backtracking.

    In combination problems, we often use:

    ```python
    backtrack(startIndex)
    ```

    because we are choosing elements from a candidate list.

    For example:

    ```text
    LeetCode 77 — Combinations
    ```

    might use:

    ```python
    for i in range(startIndex, n):
    ```

    But N-Queens is different.

    Each recursion level already represents a specific row.

    The structure is:

    ```text
    recursion level → row
    for loop        → column
    ```

    Therefore:

    ```python
    def backtrack(row):
        for col in range(n):
    ```

    is enough.

    There is no need for a `startIndex`.

    The structure can be summarized as:

    ```python
    def backtrack(row):
        if all rows are processed:
            save solution
            return

        for col in every column:
            if position is valid:
                place queen

                backtrack(row + 1)

                remove queen
    ```

    For this problem:

    ```python
    if self.isValid(row, j, board, n):
        board[row][j] = 'Q'

        self.backtrack(row + 1, n, board)

        board[row][j] = '.'
    ```

    There are at most `n` choices for the first row, then fewer choices for later rows because many positions become invalid.

    A commonly used upper bound for the backtracking search is:

    ```text
    O(n!)
    ```

    because each row can use a different column.

    In this implementation, however, `isValid()` also scans columns and diagonals, costing up to:

    ```text
    O(n)
    ```

    per validity check.

    So the practical complexity includes this additional checking cost.

    The board uses:

    ```text
    O(n²)
    ```

    space.

    The recursion depth is:

    ```text
    O(n)
    ```



    ### One recursion level represents one row

    ```text
    backtrack(row)
    ```

    means:

    > Find a valid column for the queen in the current row.
    >

    ### The `for` loop represents column choices

    ```python
    for j in range(n):
    ```

    means:

    > Try placing the queen in every column of the current row.
    >

    Therefore:

    ```text
    recursion → rows
    for loop  → columns
    ```

    ### Only check previously placed queens

    Since queens are placed from top to bottom, we only need to check:

    ```text
    ↑   same column
    ↖   upper-left diagonal
    ↗   upper-right diagonal
    ```

    There is no need to check the lower part of the board.

    ### Standard backtracking structure

    ```text
    Choose
    ↓
    Explore
    ↓
    Undo
    ```

    In this problem:

    ```python
    board[row][j] = 'Q'

    self.backtrack(row + 1, n, board)

    board[row][j] = '.'
    ```

    ### `isValid()` is the pruning step

    Without validity checking, we would explore many impossible boards.

    `isValid()` immediately removes branches where the new queen would be attacked.

    Therefore:

    ```text
    Backtracking
    =
    DFS
    +
    Pruning
    +
    Undoing choices
    ```

    The most important sentence for this problem is:

    > **Each recursion level fixes one row, and the loop tries every possible column in that row.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 5, 2026 |

## Python solution

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        self.res = []
        self.backtrack(0, n, [['.'] * n for _ in range(n)])
        return self.res

    def backtrack(self, row, n, board):
        if row == n:
            self.res.append([''.join(r) for r in board])
            return

        for j in range(n):
            if self.isValid(row, j, board, n):
                board[row][j] = 'Q'

                self.backtrack(row + 1, n, board)

                board[row][j] = '.'

    def isValid(self, row, col, board, n):
        # 1. Check the same column
        for i in range(n):
            if board[i][col] == 'Q':
                return False

        # 2. Check upper-left diagonal
        rowIdx, colIdx = row - 1, col - 1

        while rowIdx >= 0 and colIdx >= 0:
            if board[rowIdx][colIdx] == 'Q':
                return False

            rowIdx -= 1
            colIdx -= 1

        # 3. Check upper-right diagonal
        rowIdx, colIdx = row - 1, col + 1

        while rowIdx >= 0 and colIdx < n:
            if board[rowIdx][colIdx] == 'Q':
                return False

            rowIdx -= 1
            colIdx += 1

        return True
```

---

[Back to Day 22 Backtrack IV](index.md) · [All Algorithm notes](../index.md)
