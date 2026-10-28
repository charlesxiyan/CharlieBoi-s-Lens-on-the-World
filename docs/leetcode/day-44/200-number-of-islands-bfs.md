---
hide:
  - toc
---

# 200. Number of Islands (BFS)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/number-of-islands/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given a 2D grid consisting of `'1'` (land) and `'0'` (water), count the number of islands.

    An island is formed by **horizontally or vertically adjacent** land cells. The grid is completely surrounded by water.

## Python solution

```python
from collections import deque

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        res = 0

        visit = [[False] * n for _ in range(m)]

        def bfs(i, j):
            queue = deque([(i, j)])
            while queue:
                p, q = queue.popleft()
                if 0 <= p < m and 0 <= q < n and grid[p][q] == '1' and not visit[p][q]:
                    visit[p][q] = True
                    queue.extend([(p - 1, q), (p + 1, q), (p, q - 1), (p, q + 1)])

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1' and not visit[i][j]:
                    res += 1
                    bfs(i, j)
        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given a 2D grid consisting of `'1'` (land) and `'0'` (water), count the number of islands.

    An island is formed by **horizontally or vertically adjacent** land cells. The grid is completely surrounded by water.

    For example:

    ```python
    grid = [
        ["1","1","0","0","0"],
        ["1","1","0","0","0"],
        ["0","0","1","0","0"],
        ["0","0","0","1","1"]
    ]
    ```

    Output:

    ```python
    3
    ```

    The key observation is:

    > Every time we discover an unvisited land cell (`'1'`), we have found a **new island**.
    >

    Instead of using DFS, this solution uses **Breadth-First Search (BFS)** to visit every land cell belonging to the same island.

    BFS explores the graph **level by level**.

    Starting from one land cell, it visits all connected land cells using a queue.

    Every visited land cell is marked in the `visit` array so that it will never be processed again.

    Thus,

    - Every island is counted exactly once.
    - Every land cell is visited exactly once.

    Unlike the DFS solution, this implementation **does not modify the original grid**.

    Instead, it uses

    ```python
    visit[i][j]
    ```

    to record whether a cell has already been explored.

    Advantages:

    - The original grid remains unchanged.
    - Easy to understand.
    - Useful when the input grid cannot be modified.

    The BFS starts from one land cell.

    ```python
    queue = deque([(i, j)])
    ```

    Each iteration removes one position from the queue.

    ```python
    p, q = queue.popleft()
    ```

    Before processing the cell, verify that

    - it is inside the grid,
    - it is land (`'1'`),
    - it has not been visited.

    ```python
    if 0 <= p < m and 0 <= q < n and grid[p][q] == '1' and not visit[p][q]:
    ```

    If all conditions are satisfied,

    mark it as visited.

    ```python
    visit[p][q] = True
    ```

    Then push its four neighboring cells into the queue.

    ```python
    queue.extend([
        (p - 1, q),
        (p + 1, q),
        (p, q - 1),
        (p, q + 1)
    ])
    ```

    Eventually, every land cell in the same island will be visited.

    Traverse every cell in the grid.

    Whenever an unvisited land cell is found,

    ```python
    grid[i][j] == '1' and not visit[i][j]
    ```

    we have discovered a new island.

    Increase the answer.

    ```python
    res += 1
    ```

    Then perform BFS starting from this cell.

    ```python
    bfs(i, j)
    ```

    The BFS marks the entire island as visited.

    Consider

    ```python
    grid = [
        ["1","1","0"],
        ["0","1","0"],
        ["1","0","1"]
    ]
    ```

    The grid is

    ```text
    1 1 0
    0 1 0
    1 0 1
    ```

    Initially,

    ```text
    Result = 0

    Visited

    F F F
    F F F
    F F F
    ```

    ## Scan Row 0

    The first unvisited land cell is

    ```text
    (0,0)
    ```

    Increase the answer.

    ```text
    Result = 1
    ```

    Initialize the queue.

    ```text
    Queue = [(0,0)]
    ```

    ### BFS Step 1

    Pop

    ```text
    (0,0)
    ```

    Mark it as visited.

    Push its four neighbors.

    ```text
    Queue

    (-1,0)
    (1,0)
    (0,-1)
    (0,1)
    ```

    Invalid positions will simply be ignored when popped.

    ### BFS Step 2

    Eventually,

    ```text
    (0,1)
    ```

    is processed.

    It is land.

    Mark it visited.

    Push its neighbors.

    ### BFS Step 3

    Next,

    ```text
    (1,1)
    ```

    is processed.

    It is also land.

    After the queue becomes empty,

    the first island has been completely explored.

    Visited becomes

    ```text
    T T F
    F T F
    F F F
    ```

    ## Continue Scanning

    The next unvisited land is

    ```text
    (2,0)
    ```

    Increase

    ```text
    Result = 2
    ```

    Run BFS.

    Visited becomes

    ```text
    T T F
    F T F
    T F F
    ```

    ## Continue Scanning

    The next unvisited land is

    ```text
    (2,2)
    ```

    Increase

    ```text
    Result = 3
    ```

    Run BFS.

    Final visited array

    ```text
    T T F
    F T F
    T F T
    ```

    Traversal finishes.

    Final answer:

    ```python
    3
    ```

    Notice that this implementation pushes all four neighboring coordinates directly.

    ```python
    queue.extend([
        (p - 1, q),
        (p + 1, q),
        (p, q - 1),
        (p, q + 1)
    ])
    ```

    Some of these coordinates may

    - go outside the grid,
    - be water,
    - or have already been visited.

    Instead of checking these conditions before pushing,

    this implementation checks them **after popping**.

    ```python
    if valid:
        process()
    ```

    This keeps the code concise.

    Although a few invalid positions enter the queue, each one is discarded immediately after being popped, so the overall time complexity remains **O(m × n)**.

    **Time Complexity**

    ```text
    O(m × n)
    ```

    Every cell is processed at most once.

    **Space Complexity**

    ```text
    O(m × n)
    ```

    The `visit` array requires `O(m × n)` space.

    The queue may also contain `O(m × n)` positions in the worst case.

    **Algorithm**

    ```text
    Breadth-First Search (BFS)
    ```

    **Queue Content**

    ```text
    Grid coordinates (row, column)
    ```

    **Visited Record**

    ```python
    visit[row][col]
    ```

    **Search Directions**

    ```text
    Up
    Down
    Left
    Right
    ```

    **Main Idea**

    ```text
    Whenever an unvisited land cell is found,
    it represents a new island.

    Run BFS to visit every connected land cell,
    mark them as visited,
    and continue scanning the grid.
    ```

    ## Common Mistakes

    - Forgetting to mark a land cell as visited after processing it, causing repeated visits.
    - Forgetting to check whether a cell has already been visited before starting a new BFS.
    - Using diagonal directions. Only the four cardinal directions are considered connected.
    - Incrementing the answer inside the BFS. The answer should increase only once when a new island is first discovered.
    - Confusing this implementation with the DFS version. Here, the original grid is **not modified**; the `visit` array is responsible for recording visited cells.
    - Trying to validate neighbors before pushing them into the queue is unnecessary in this implementation. Invalid coordinates are safely ignored when they are popped from the queue.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 18, 2026 |

---

[Back to Day 44 Graph II](index.md) · [All Algorithm notes](../index.md)
