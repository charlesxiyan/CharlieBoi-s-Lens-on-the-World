---
hide:
  - toc
---

# 79. Word Search

*Edited by Charles*

**Medium · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/word-search/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Use **DFS + Backtracking**.

    Each cell in the board can be the starting point of the word.

## Python solution

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        self.directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        self.m, self.n = len(board), len(board[0])
        self.res = False

        for i in range(self.m):
            for j in range(self.n):
                if board[i][j] == word[0]:
                    temp, board[i][j] = board[i][j], '.'
                    self.backtrack(i, j, 1, board, word)
                    board[i][j] = temp

                    if self.res:
                        return True

        return False

    def backtrack(self, row, col, startIndex, board, word):
        if startIndex == len(word):
            self.res = True
            return

        for dir in self.directions:
            nextRow = row + dir[0]
            nextCol = col + dir[1]

            if (
                0 <= nextRow < self.m
                and 0 <= nextCol < self.n
                and board[nextRow][nextCol] != '.'
                and board[nextRow][nextCol] == word[startIndex]
            ):
                temp, board[nextRow][nextCol] = board[nextRow][nextCol], '.'

                self.backtrack(
                    nextRow,
                    nextCol,
                    startIndex + 1,
                    board,
                    word
                )

                board[nextRow][nextCol] = temp
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **DFS + Backtracking**.

    Each cell in the board can be the starting point of the word.

    If `board[i][j] == word[0]`, we:

    1. Mark the starting cell as visited.
    2. Start DFS from `(i, j)`.
    3. Search its four neighboring cells for the next character.
    4. Mark a selected cell as visited before recursion.
    5. Restore the cell after recursion (**backtracking**).

    A cell cannot be used more than once in the same path.

    ## Backtracking State

    Define:

    ```python
    backtrack(row, col, startIndex)
    ```

    where:

    - `(row, col)` is the **current position**.
    - `word[0:startIndex]` has already been matched.
    - We now search the neighbors of `(row, col)` for `word[startIndex]`.

    For example, if:

    ```text
    word = "ABCCED"
    ```

    and we have already found `A`:

    ```text
    A
    ↑ current position
    ```

    then we call:

    ```python
    backtrack(row, col, 1)
    ```

    because `word[0] = 'A'` has already been matched, and we are now looking for:

    ```python
    word[1] = 'B'
    ```

    ## Starting Point

    Since the word can start from **any cell**, iterate through the entire board:

    ```python
    for i in range(m):
        for j in range(n):
            if board[i][j] == word[0]:
                ...
    ```

    Once `word[0]` is found, mark that cell as visited and start searching for `word[1]`:

    ```python
    board[i][j] = '.'
    backtrack(i, j, 1)
    ```

    ## Search Four Directions

    From the current position, search:

    ```text
            Up
             ↑
    Left ← Current → Right
             ↓
            Down
    ```

    For each neighbor, check:

    1. It is inside the board.
    2. It has not been visited.
    3. Its character equals `word[startIndex]`.

    ```python
    if (
        0 <= nextRow < m
        and 0 <= nextCol < n
        and board[nextRow][nextCol] != '.'
        and board[nextRow][nextCol] == word[startIndex]
    ):
    ```

    If all conditions are satisfied, continue searching from that cell.

    ## Backtracking

    Before entering a cell, temporarily mark it as visited:

    ```python
    temp = board[nextRow][nextCol]
    board[nextRow][nextCol] = '.'
    ```

    Then recursively search for the next character:

    ```python
    backtrack(nextRow, nextCol, startIndex + 1)
    ```

    After the recursive search finishes, restore the cell:

    ```python
    board[nextRow][nextCol] = temp
    ```

    This is the key **backtracking** step:

    ```text
    Choose
      ↓
    Mark as visited
      ↓
    DFS
      ↓
    Restore
    ```

    Restoring the cell allows it to be used again in a **different search path**.

    ## Base Case

    When:

    ```python
    startIndex == len(word)
    ```

    all characters have already been matched.

    Therefore:

    ```python
    self.res = True
    ```

    ## Example

    For:

    ```text
    board =
    A B C E
    S F C S
    A D E E

    word = "ABCCED"
    ```

    The search path is:

    ```text
    A → B → C
            ↓
            C
            ↓
    D ← E
    ```

    The recursive states are approximately:

    ```text
    Find A outside DFS
        ↓
    backtrack(A, 1)   # search for B
        ↓
    backtrack(B, 2)   # search for C
        ↓
    backtrack(C, 3)   # search for C
        ↓
    backtrack(C, 4)   # search for E
        ↓
    backtrack(E, 5)   # search for D
        ↓
    backtrack(D, 6)
        ↓
    startIndex == len(word)
        ↓
    Found
    ```

    ## Complexity

    Let:

    - `m × n` be the size of the board.
    - `L` be the length of `word`.

    Each cell can be used as a starting point, and from each position we can explore up to four directions.

    **Time Complexity:**

    ```text
    O(m × n × 4^L)
    ```

    A tighter bound can be written as `O(m × n × 3^L)` because after the first move, we cannot immediately return to the previous cell.

    **Space Complexity:**

    ```text
    O(L)
    ```

    The maximum recursion depth is the length of the word. The board itself is used to record visited cells, so no additional `visited` matrix is needed.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 7, 2026 |

---

[Back to Bonus Backtrack](index.md) · [All Algorithm notes](../index.md)
