---
hide:
  - toc
---

# 1091. Shortest Path in Binary Matrix (A Star)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/shortest-path-in-binary-matrix/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    See the Python implementation and detailed notes below.

## Python solution

```python
import heapq

class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        self.l = len(grid)

        if grid[0][0] or grid[self.l - 1][self.l - 1]:
            return -1

        moves = [
            (-1, 0),
            (1, 0),
            (1, 1),
            (-1, -1),
            (0, 1),
            (0, -1),
            (1, -1),
            (-1, 1)
        ]

        minHeap = [
            (1 + self.dist((0, 0)), 1, (0, 0))
        ]

        minDist = [
            [float('inf')] * self.l
            for _ in range(self.l)
        ]
        minDist[0][0] = 1

        while minHeap:
            curDist, steps, curNode = heapq.heappop(minHeap)

            if steps > minDist[curNode[0]][curNode[1]]:
                continue

            if curNode == (self.l - 1, self.l - 1):
                return steps

            for move in moves:
                nextNode = (
                    curNode[0] + move[0],
                    curNode[1] + move[1]
                )

                if (
                    0 <= nextNode[0] < self.l
                    and 0 <= nextNode[1] < self.l
                    and not grid[nextNode[0]][nextNode[1]]
                ):
                    newSteps = steps + 1

                    if newSteps < minDist[nextNode[0]][nextNode[1]]:
                        minDist[nextNode[0]][nextNode[1]] = newSteps

                        newLen = newSteps + self.dist(nextNode)

                        heapq.heappush(
                            minHeap,
                            (newLen, newSteps, nextNode)
                        )

        return -1

    def dist(self, point):
        return max(
            abs(self.l - 1 - point[0]),
            abs(self.l - 1 - point[1])
        )
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: A* Search

    ### Core Idea

    We need to find the shortest clear path from:

    ```text
    (0, 0)
    ```

    to:

    ```text
    (n - 1, n - 1)
    ```

    A cell with value `0` can be visited, while a cell with value `1` is blocked.

    From each cell, we can move in **8 directions**:

    ```text
    ↖  ↑  ↗
    ←     →
    ↙  ↓  ↘
    ```

    Since every move has the same cost, BFS can solve this problem.

    However, we can also use **A\*** to guide the search toward the destination and potentially explore fewer unnecessary cells.

    ## A* Search

    A* uses three important values:

    ```text
    g(n) = actual cost from the start to the current node

    h(n) = estimated cost from the current node to the goal

    f(n) = g(n) + h(n)
    ```

    For this problem:

    ```text
    Start = (0, 0)
    Goal  = (n - 1, n - 1)
    ```

    We can visualize them as:

    ```text
    Start ----------------> Current ----------------> Goal
              g(n)                     h(n)

    f(n) = g(n) + h(n)
    ```

    ### Meaning of Each Value

    `g(n)` is the actual path length we have already traveled.

    `h(n)` estimates how far the current cell is from the destination.

    `f(n)` estimates the total length of a path that goes through the current cell.

    A* uses:

    ```text
    f(n)
    ```

    as the priority in the min-heap.

    Therefore, A* prefers nodes that have:

    ```text
    small actual cost
    +
    small estimated remaining cost
    ```

    ## Why Use Chebyshev Distance?

    The problem allows movement in 8 directions.

    For a current cell:

    ```text
    (r, c)
    ```

    and the goal:

    ```text
    (n - 1, n - 1)
    ```

    the row difference is:

    ```text
    n - 1 - r
    ```

    and the column difference is:

    ```text
    n - 1 - c
    ```

    Since diagonal movement is allowed, one move can reduce both the row difference and the column difference at the same time.

    Therefore, if there were no obstacles, the minimum number of moves required is:

    ```text
    max(rowDifference, columnDifference)
    ```

    This is the **Chebyshev Distance**.

    We implement it as:

    ```python
    def dist(self, point):
        return max(
            abs(self.l - 1 - point[0]),
            abs(self.l - 1 - point[1])
        )
    ```

    For example:

    ```text
    Current = (0, 0)
    Goal    = (4, 2)

    row difference = 4
    col difference = 2
    ```

    We can move diagonally twice:

    ```text
    (0,0)
       ↘
       (1,1)
          ↘
          (2,2)
    ```

    and then vertically twice:

    ```text
    (2,2)
      ↓
    (3,2)
      ↓
    (4,2)
    ```

    Total:

    ```text
    4 moves
    ```

    which is:

    ```text
    max(4, 2) = 4
    ```

    Therefore:

    ```text
    h(n) = Chebyshev Distance
    ```

    is a natural heuristic for this problem.

    ## Why Is Chebyshev Distance a Valid Heuristic?

    The heuristic should not overestimate the actual remaining shortest-path cost.

    Chebyshev Distance calculates the shortest possible distance when we **ignore all obstacles**.

    Without obstacles:

    ```text
    h(n) = minimum possible remaining moves
    ```

    Adding obstacles can only make the actual path equal or longer:

    ```text
    Chebyshev Distance
            <=
    Actual Remaining Distance
    ```

    Therefore, the heuristic does not overestimate the actual cost.

    This makes it suitable for A*.

    ## Why Not Use Manhattan Distance?

    Manhattan Distance is:

    ```text
    |row difference| + |column difference|
    ```

    It is natural when movement is only allowed in 4 directions:

    ```text
       ↑
    ←     →
       ↓
    ```

    But this problem allows diagonal movement.

    For example:

    ```text
    Start = (0, 0)
    Goal  = (2, 2)
    ```

    The actual minimum number of moves is:

    ```text
    (0,0)
       ↘
       (1,1)
          ↘
          (2,2)

    2 moves
    ```

    But Manhattan Distance gives:

    ```text
    2 + 2 = 4
    ```

    which overestimates the true minimum cost.

    Therefore:

    ```text
    4 directions
    → Manhattan Distance

    8 directions with equal move cost
    → Chebyshev Distance
    ```

    ## Initial Check

    If either the start or the destination is blocked:

    ```python
    if grid[0][0] or grid[self.l - 1][self.l - 1]:
        return -1
    ```

    there cannot be a valid path.

    Therefore, we immediately return:

    ```text
    -1
    ```

    ## Eight Possible Moves

    We define all 8 directions:

    ```python
    moves = [
        (-1, 0),
        (1, 0),
        (1, 1),
        (-1, -1),
        (0, 1),
        (0, -1),
        (1, -1),
        (-1, 1)
    ]
    ```

    For the current node:

    ```text
    (row, col)
    ```

    the next node is:

    ```python
    nextNode = (
        curNode[0] + move[0],
        curNode[1] + move[1]
    )
    ```

    ## Define `minDist`

    We use:

    ```python
    minDist = [
        [float('inf')] * self.l
        for _ in range(self.l)
    ]
    ```

    where:

    ```text
    minDist[r][c]
    ```

    represents:

    > The minimum actual path length `g` found so far from `(0, 0)` to `(r, c)`.
    >

    It stores `g`, **not `f`**.

    This distinction is very important.

    ```text
    minDist → g
    dist()  → h
    heap priority → f = g + h
    ```

    ## Why Does the Starting Distance Equal 1?

    We initialize:

    ```python
    minDist[0][0] = 1
    ```

    because the problem defines path length as the **number of cells in the path**, not the number of edges.

    For example:

    ```text
    (0,0) → (1,1) → (2,2)
    ```

    contains:

    ```text
    3 cells
    ```

    so the path length is:

    ```text
    3
    ```

    Therefore, at the starting cell:

    ```text
    g(start) = 1
    ```

    rather than `0`.

    ## Initialize the Min-Heap

    The heap stores:

    ```text
    (f, g, node)
    ```

    In the code:

    ```python
    minHeap = [
        (1 + self.dist((0, 0)), 1, (0, 0))
    ]
    ```

    At the starting node:

    ```text
    g = 1

    h = self.dist((0, 0))

    f = g + h
    ```

    Therefore:

    ```python
    (
        1 + self.dist((0, 0)),   # f
        1,                        # g
        (0, 0)                    # node
    )
    ```

    ## Pop the Most Promising Node

    We pop:

    ```python
    curDist, steps, curNode = heapq.heappop(minHeap)
    ```

    In this code:

    ```text
    curDist = f = g + h
    steps   = g
    curNode = current cell
    ```

    The variable name `curDist` here represents the A* priority `f`.

    The actual path length is:

    ```text
    steps
    ```

    This distinction is important.

    ## Skip Stale States

    A node may be pushed into the heap multiple times.

    For example, suppose we first find:

    ```text
    g(A) = 8
    ```

    and push it into the heap.

    Later, we find a better path:

    ```text
    g(A) = 5
    ```

    and push it again.

    The heap now contains both states:

    ```text
    (..., 5, A)
    (..., 8, A)
    ```

    But:

    ```python
    minDist[A] = 5
    ```

    When the old state with `g = 8` is eventually popped, we skip it:

    ```python
    if steps > minDist[curNode[0]][curNode[1]]:
        continue
    ```

    Notice that we compare:

    ```text
    g vs minDist
    ```

    not:

    ```text
    f vs minDist
    ```

    because `minDist` stores actual distances `g`.

    ## Why Return `steps`?

    When we reach:

    ```python
    if curNode == (self.l - 1, self.l - 1):
    ```

    we return:

    ```python
    return steps
    ```

    because `steps` is:

    ```text
    g(goal)
    ```

    which is the actual shortest path length from the start to the goal.

    Remember:

    ```text
    g = actual cost
    h = estimated remaining cost
    f = g + h = search priority
    ```

    The problem asks for the actual shortest path length, so conceptually the answer is `g`.

    At the goal:

    ```text
    h(goal) = 0
    ```

    so:

    ```text
    f(goal)
    = g(goal) + h(goal)
    = g(goal)
    ```

    Thus `f` and `g` happen to have the same value at the goal, but returning `g` is conceptually clearer.

    ## Relaxation

    For every valid neighbor:

    ```python
    newSteps = steps + 1
    ```

    This calculates the new actual distance:

    ```text
    newG = currentG + 1
    ```

    because moving to a neighboring cell costs one additional step.

    If:

    ```python
    newSteps < minDist[nextNode[0]][nextNode[1]]
    ```

    we have found a better path to `nextNode`.

    Therefore:

    ```python
    minDist[nextNode[0]][nextNode[1]] = newSteps
    ```

    ## Calculate the New A* Priority

    After obtaining the new actual distance:

    ```text
    newG = newSteps
    ```

    we calculate:

    ```python
    newLen = newSteps + self.dist(nextNode)
    ```

    which means:

    ```text
    newF = newG + newH
    ```

    where:

    ```text
    newG = actual distance from start to nextNode

    newH = estimated distance from nextNode to goal

    newF = estimated total path cost
    ```

    Then:

    ```python
    heapq.heappush(
        minHeap,
        (newLen, newSteps, nextNode)
    )
    ```

    Again, the heap stores:

    ```text
    (f, g, node)
    ```

    ## The Most Important Part of A*

    The core process can be summarized as:

    ```text
    Current Node
         |
         | actual movement cost = 1
         ↓
    Next Node
         |
         ├── newG = currentG + 1
         |
         ├── newH = heuristic(nextNode)
         |
         └── newF = newG + newH
                        |
                        ↓
                   Push to Heap
    ```

    In code:

    ```python
    newSteps = steps + 1

    if newSteps < minDist[nextNode[0]][nextNode[1]]:
        minDist[nextNode[0]][nextNode[1]] = newSteps

        newLen = newSteps + self.dist(nextNode)

        heapq.heappush(
            minHeap,
            (newLen, newSteps, nextNode)
        )
    ```

    ## A* vs Dijkstra

    Dijkstra uses:

    ```text
    priority = g
    ```

    It only considers the actual distance traveled so far.

    A* uses:

    ```text
    priority = g + h
    ```

    It considers both:

    ```text
    distance already traveled
    +
    estimated distance remaining
    ```

    Therefore:

    ```text
    Dijkstra:

    Start → expands outward based on g

    A*:

    Start → searches more toward the Goal
            based on g + h
    ```

    The heuristic gives A* a sense of direction.

    ## A* vs Greedy Best-First Search

    There are three related strategies:

    | Algorithm | Priority |
    | --- | --- |
    | Dijkstra | `g` |
    | Greedy Best-First Search | `h` |
    | A* | `g + h` |

    ### Dijkstra

    ```text
    priority = g
    ```

    Only considers how much cost has already been paid.

    ### Greedy Best-First Search

    ```text
    priority = h
    ```

    Only considers how close a node appears to be to the destination.

    This may aggressively move toward the goal but can choose a bad route because it ignores the cost already paid.

    ### A*

    ```text
    priority = g + h
    ```

    Balances both:

    ```text
    actual cost so far
    +
    estimated remaining cost
    ```

    This is why A* can be both goal-directed and still find the shortest path when using an appropriate heuristic.

    ## Complexity

    There are at most:

    ```text
    n²
    ```

    cells.

    Each cell has at most:

    ```text
    8
    ```

    neighbors.

    Heap operations take:

    ```text
    O(log(n²)) = O(log n)
    ```

    Therefore, a standard upper-bound analysis is:

    ```text
    Time: O(n² log n)
    ```

    The distance matrix and heap may contain up to `O(n²)` states:

    ```text
    Space: O(n²)
    ```

    ### Remember the three values in A*

    ```text
    g = actual distance from Start to Current

    h = estimated distance from Current to Goal

    f = g + h
    ```

    In this implementation:

    ```text
    steps        → g

    self.dist()  → h

    curDist      → f

    minDist[][]  → best known g
    ```

    ### The heap is ordered by `f`

    ```python
    heapq.heappush(
        minHeap,
        (f, g, node)
    )
    ```

    A* uses `f` to decide which node should be explored first.

    ### Relaxation must use `g`

    When moving to a neighbor:

    ```python
    newSteps = steps + 1
    ```

    which means:

    ```text
    newG = currentG + edgeCost
    ```

    Do **not** use:

    ```text
    newG = currentF + edgeCost
    ```

    because `f` already contains the heuristic.

    ### `minDist` stores `g`, not `f`

    ```text
    minDist[r][c]
    =
    minimum actual path length found so far
    from the start to (r, c)
    ```

    Therefore, the stale-state check is:

    ```python
    if steps > minDist[row][col]:
        continue
    ```

    ### For 8-direction movement, use Chebyshev Distance

    ```python
    h = max(rowDifference, colDifference)
    ```

    because diagonal movement can reduce both differences simultaneously.

    A useful rule to remember:

    ```text
    4-direction grid
    → Manhattan Distance

    8-direction grid with equal move cost
    → Chebyshev Distance
    ```

    ### The central idea of A*

    ```text
    Dijkstra:
    priority = g

    A*:
    priority = g + h
    ```

    The heuristic `h` helps A* focus the search toward the destination instead of expanding equally in every direction.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 3, 2026 |

---

[Back to Day 55 Graph XIII](index.md) · [All Algorithm notes](../index.md)
