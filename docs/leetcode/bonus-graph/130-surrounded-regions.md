---
hide:
  - toc
---

# 130. Surrounded Regions

*Edited by Charles*

**Medium · Python · Graph · Boundary DFS**

## 1. Problem Description

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/surrounded-regions/description/).

    Given a rectangular board of `X` and `O` cells, change every surrounded
    region of `O` cells into `X`, modifying the board in place.

    Cells connect only through their four sides. An `O` region is surrounded
    exactly when none of its cells can reach an `O` on the boundary.
    Return nothing; keep boundary-connected regions unchanged.

## 2. Approach and Core Idea

!!! tip "Protect the cells that cannot be captured"
    Start DFS at every boundary `O`. Mark every reachable `O` as `T`.
    Afterward, an `O` that is still unmarked must be enclosed.

Think of a boundary `O` as an escape route. Every `O` connected to it also has
an escape route, even if that cell is far from the boundary.

This gives a useful reversal: instead of searching for every enclosed region,
find all the cells that are **not enclosed** first.

Counting connected components alone cannot tell us which cells to capture:
we need to know whether each component touches the boundary. A component-by-component
solution can work, but it needs that extra tracking. Boundary DFS directly marks
all safe cells, and the board itself stores the classification.

**Why it works:** DFS marks exactly the `O` cells reachable from boundary `O`s.
Those cells cannot be surrounded. Every remaining `O` belongs to a region with
no boundary connection, so it must be captured.

## 3. Algorithm Steps

1. If the board is empty, stop.
2. Scan the first and last columns, then the first and last rows.
3. Whenever a boundary cell is `O`, run DFS and change its entire reachable
   `O` region to `T`.
4. Scan every cell once more:
   - Remaining `O` → `X`: capture it.
   - `T` → `O`: restore the protected region.
   - `X` stays unchanged.

## 4. Hand Simulation

Consider this board; coordinates are zero-based:

```text
X X X X
X O O X
X X O X
X O X X
```

**Boundary scan:** the only boundary `O` is at `(3, 1)`. Its four neighbors
either lie outside the board or contain `X`, so DFS marks only that cell:

```text
X X X X
X O O X
X X O X
X T X X
```

**Capture:** `(1, 1)`, `(1, 2)`, and `(2, 2)` still contain `O`.
They are connected to each other but cannot reach any boundary `O`, so all three
become `X`.

**Restore:** change `(3, 1)` from `T` back to `O`:

```text
X X X X
X X X X
X X X X
X O X X
```

The method protects whole regions, not just boundary cells: if the bottom `O`
had an adjacent chain of `O`s leading inward, DFS would mark that entire chain.

## 5. Important Implementation Details

- **Mark before pushing onto the stack.** This prevents a cell from being
  scheduled repeatedly through different neighbors.
- **Use a stack for iterative DFS.** A long region can exceed Python's default
  recursion depth. An explicit stack keeps the same DFS idea without recursive calls.
- **Do not mark `X` cells.** They block connectivity.
- **Repeated corner checks are harmless.** A previously visited cell is already
  `T`, so a later boundary check skips it.
- **Restore only after all boundary searches finish.** Restoring early would
  discard the visited markers.
- **One-row or one-column boards work too.** Every `O` is boundary-connected,
  so no `O` is captured.

## 6. Time and Space Complexity

Let $m$ be the number of rows and $n$ the number of columns.

| Resource | Complexity | Reason |
| --- | --- | --- |
| Time | $O(mn)$ | Each cell is marked at most once; the final scan visits all cells. |
| Auxiliary space | $O(mn)$ worst case | The explicit DFS stack can contain a linear number of cells. |

The board is updated in place, but **in-place mutation does not mean constant
auxiliary space**: the stack still uses memory.

## 7. Complete Python Solution

```python
from typing import List


class Solution:
    def solve(self, board: List[List[str]]) -> None:
        if not board or not board[0]:
            return

        rows, cols = len(board), len(board[0])
        directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

        def dfs(row: int, col: int) -> None:
            if board[row][col] != "O":
                return

            board[row][col] = "T"
            stack = [(row, col)]

            while stack:
                r, c = stack.pop()
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc
                    if (
                        0 <= nr < rows
                        and 0 <= nc < cols
                        and board[nr][nc] == "O"
                    ):
                        board[nr][nc] = "T"
                        stack.append((nr, nc))

        for row in range(rows):
            dfs(row, 0)
            dfs(row, cols - 1)

        for col in range(cols):
            dfs(0, col)
            dfs(rows - 1, col)

        for row in range(rows):
            for col in range(cols):
                if board[row][col] == "O":
                    board[row][col] = "X"
                elif board[row][col] == "T":
                    board[row][col] = "O"
```

## 8. Key Takeaways

- Boundary connectivity determines whether a region is safe; its size does not.
- Mark safe cells first, then capture everything left over.
- A temporary marker can serve as both visited state and classification.
- Iterative DFS is useful when a recursive search could become too deep.

---

[Back to Bonus Graph](index.md) · [All Algorithm notes](../index.md)
