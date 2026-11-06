---
hide:
  - toc
---

# 427. Construct Quad Tree

*Edited by Charles*

**Medium · Python · Quad Tree · Divide and Conquer**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/construct-quad-tree/description/).

    Represent a square binary grid as a quad tree. Its side length is a positive power of two. A uniform region becomes a leaf; a mixed region is split into four equal quadrants with children in top-left, top-right, bottom-left, bottom-right order.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    `dfs(row, col, size)` represents the square starting at `(row, col)` with side
    length `size`. Scan its values. If they all match, compress the entire square
    into one leaf. Otherwise, recursively construct its four quadrants.

    This is a quad-tree exercise, included in this bonus module alongside the linked-list notes.

    ## Algorithm Steps

    1. Read the region's first value into `val` and initially set `same = True`.
    2. Scan the square. On any mismatch, set `same = False` and exit both loops.
    3. For a uniform region, return a leaf with value `bool(val)`.
    4. Otherwise set `half = size // 2`.
    5. Recursively construct the four children using the corresponding offsets.
    6. Return a non-leaf node containing those children.

    ## Hand Simulation

    Use this custom grid:

    ```text
    1 1 | 0 0
    1 1 | 0 0
    ----+----
    1 1 | 0 1
    1 1 | 1 0
    ```

    The full region is mixed, so `dfs(0, 0, 4)` splits into size-two quadrants:

    | Child | Call | Result |
    | --- | --- | --- |
    | Top left | `dfs(0, 0, 2)` | A leaf with value `True`. |
    | Top right | `dfs(0, 2, 2)` | A leaf with value `False`. |
    | Bottom left | `dfs(2, 0, 2)` | A leaf with value `True`. |
    | Bottom right | `dfs(2, 2, 2)` | Mixed; split again. |

    The bottom-right region produces four size-one leaves with values
    `False, True, True, False` in the same quadrant order. Each one-cell region is
    necessarily uniform, so recursion stops naturally without a separate size-one case.

    ## Important Implementation Details

    - `range(row, row + size)` and its column equivalent scan exactly the current
      square; Python excludes the upper endpoints.
    - The inner `break` exits only the column loop. The subsequent `if not same`
      also exits the row loop to stop scanning after a mismatch.
    - Row offsets move downward; column offsets move rightward. In particular,
      bottom-left adds `half` only to `row`, while top-right adds it only to `col`.
    - The grid is accessed by coordinates without slicing or modification.
    - For an internal node, `val` may be either Boolean value. `Node(True, False, ...)`
      is valid because its children represent the region's actual contents.

    ### Node construction when running locally

    The supplied algorithm calls `Node(bool(val), True)` for a leaf, so the runtime
    `Node` constructor must default its four child arguments to `None`. The illustrative
    class in the supplied comment lists all six arguments as required; copying that
    comment verbatim into an executable class would therefore raise `TypeError`.
    For a local harness, use the compatible signature
    `Node(val, isLeaf, topLeft=None, topRight=None, bottomLeft=None, bottomRight=None)`.
    Also provide `List` from `typing`. The main code below preserves Charles's
    implementation and its original class comment.

    **Why it works:** a leaf exactly represents a uniform square. For a mixed square,
    the four disjoint quadrants cover it completely. By recursively representing
    each quadrant correctly and attaching them in order, the parent represents the
    original region exactly.

    ## Time and Space Complexity

    Let $n$ be the grid side length. A region of side $s$ scans at most $s^2$ cells
    before it splits. A conservative recurrence is
    $T(n)\le4T(n/2)+O(n^2)$, yielding an $O(n^2\log n)$ time upper bound for $n\ge2$.
    Early mismatch detection can substantially reduce scanning; this bound does not
    claim every input scans every cell at every level. A uniform grid takes $O(n^2)$
    time and creates one node.

    The recursion depth is $O(\log n)$, using that much auxiliary stack space
    ($O(1)$ for a one-cell grid). The returned tree can contain $O(n^2)$ nodes,
    which is output storage. No subgrid copies are created.

    ## Key Takeaways

    Compress uniform regions and subdivide only mixed ones. Keep quadrant offsets
    and child order consistent, and distinguish recursion space from returned-tree space.

## Python solution

```python
"""
# Definition for a QuadTree node.
class Node:
    def __init__(self, val, isLeaf, topLeft, topRight, bottomLeft, bottomRight):
        self.val = val
        self.isLeaf = isLeaf
        self.topLeft = topLeft
        self.topRight = topRight
        self.bottomLeft = bottomLeft
        self.bottomRight = bottomRight
"""

class Solution:
    def construct(self, grid: List[List[int]]) -> 'Node':
        n = len(grid)

        def dfs(row, col, size):
            val = grid[row][col]
            same = True

            for i in range(row, row + size):
                for j in range(col, col + size):
                    if grid[i][j] != val:
                        same = False
                        break

                if not same:
                    break

            if same:
                return Node(bool(val), True)

            half = size // 2

            topLeft = dfs(row, col, half)
            topRight = dfs(row, col + half, half)
            bottomLeft = dfs(row + half, col, half)
            bottomRight = dfs(row + half, col + half, half)

            return Node(True, False, topLeft, topRight, bottomLeft, bottomRight)

        return dfs(0, 0, n)
```

---

[Back to Bonus Linked List](index.md) · [All Algorithm notes](../index.md)
