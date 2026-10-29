---
hide:
  - toc
---

# 994. Rotting Oranges

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/rotting-oranges/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        You are given an `m × n` grid where:

        Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    You are given an `m × n` grid where:

    ```text
    0 = empty cell
    1 = fresh orange
    2 = rotten orange
    ```

    Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

    The four directions are:

    ```text
    up
    down
    left
    right
    ```

    Return the minimum number of minutes until no fresh orange remains.

    If some fresh orange can never become rotten, return:

    ```text
    -1
    ```

    This problem is a classic:

    ```text
    Multi-Source BFS
    +
    Level-Order BFS
    ```

    The key observation is:

    > All initially rotten oranges start spreading at the same time.
    >

    Therefore, instead of starting BFS from only one rotten orange, we put **all initially rotten oranges into the queue**.

    Suppose:

    ```text
    2 1 1 1 2
    ```

    There are two rotten oranges:

    ```text
    2       2
    ↑       ↑
    source  source
    ```

    Both of them start infecting nearby oranges at the same time.

    Therefore, we should initialize:

    ```python
    queue = deque()
    ```

    and add **every rotten orange**:

    ```python
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 2:
                queue.append((i, j))
    ```

    Now the initial queue represents:

    ```text
    all oranges that are already rotten at minute 0
    ```

    A rotten orange can infect its four neighbors:

    ```text
            (-1, 0)
                ↑
                |
    (0,-1) ← current → (0,1)
                |
                ↓
             (1, 0)
    ```

    So we define:

    ```python
    dirs = [
        (1, 0),
        (-1, 0),
        (0, 1),
        (0, -1)
    ]
    ```

    For the current orange:

    ```python
    curNode
    ```

    and one direction:

    ```python
    move
    ```

    the next position is:

    ```python
    nextNode = (
        curNode[0] + move[0],
        curNode[1] + move[1]
    )
    ```

    Before accessing:

    ```python
    grid[nextNode[0]][nextNode[1]]
    ```

    we must make sure the position is inside the grid:

    ```python
    0 <= nextNode[0] < m
    ```

    and:

    ```python
    0 <= nextNode[1] < n
    ```

    Therefore:

    ```python
    if (
        0 <= nextNode[0] < m
        and 0 <= nextNode[1] < n
        and grid[nextNode[0]][nextNode[1]] == 1
    ):
    ```

    Notice that the lower bound must be:

    ```text
    0
    ```

    not:

    ```text
    1
    ```

    because Python uses 0-based indexing.

    The spreading process happens level by level.

    For example:

    ```text
    Minute 0:

    2 1 1
    1 1 0
    0 1 1
    ```

    After one BFS level:

    ```text
    Minute 1:

    2 2 1
    2 1 0
    0 1 1
    ```

    After another BFS level:

    ```text
    Minute 2:

    2 2 2
    2 2 0
    0 1 1
    ```

    So:

    ```text
    one BFS level
    =
    one minute of spreading
    ```

    This is exactly the structure of Level-Order BFS.

    This is the most important part for counting time.

    We write:

    ```python
    while queue:
        for _ in range(len(queue)):
    ```

    At the beginning of each `while` iteration:

    ```text
    queue
    ```

    contains all rotten oranges belonging to the **current BFS level**.

    Suppose:

    ```text
    queue = [A, B, C]
    ```

    Then:

    ```python
    len(queue) = 3
    ```

    so:

    ```python
    for _ in range(len(queue)):
    ```

    processes exactly:

    ```text
    A
    B
    C
    ```

    These three oranges spread simultaneously during the current minute.

    During the loop, we may do:

    ```python
    queue.append(nextNode)
    ```

    For example:

    ```text
    Initial queue:

    [A, B, C]
    ```

    While processing `A`, we may add `D`:

    ```text
    [B, C, D]
    ```

    Then processing `B` may add `E`:

    ```text
    [C, D, E]
    ```

    But the loop was created using the original:

    ```python
    len(queue) = 3
    ```

    So only three nodes are processed during this level:

    ```text
    A
    B
    C
    ```

    The newly added:

    ```text
    D
    E
    ```

    remain in the queue for the next `while` iteration.

    Therefore:

    ```text
    current queue level
            ↓
    spread to neighbors
            ↓
    new rotten oranges enter queue
            ↓
    next BFS level
            ↓
    next minute
    ```

    This is how BFS naturally simulates time.

    When we find a fresh orange:

    ```python
    grid[nextNode[0]][nextNode[1]] == 1
    ```

    we immediately change it to:

    ```python
    grid[nextNode[0]][nextNode[1]] = 2
    ```

    and then add it to the queue:

    ```python
    queue.append(nextNode)
    ```

    So:

    ```python
    grid[nextNode[0]][nextNode[1]] = 2
    queue.append(nextNode)
    ```

    This is important because changing:

    ```text
    1 → 2
    ```

    also acts as a `visited` marker.

    Normally BFS may require:

    ```python
    visited
    ```

    to prevent the same node from being added multiple times.

    But here, when a fresh orange is discovered, we immediately do:

    ```python
    grid[r][c] = 2
    ```

    Therefore, if another rotten orange later reaches the same position, this condition:

    ```python
    grid[r][c] == 1
    ```

    will be false.

    So the orange will not be added again.

    Therefore:

    ```text
    grid[r][c] = 2
    ```

    serves two purposes:

    ```text
    1. Mark the orange as rotten
    2. Mark the cell as visited
    ```

    No extra `visited` array is necessary.

    This is a subtle but important part of this implementation.

    We initialize:

    ```python
    time = -1
    ```

    instead of:

    ```python
    time = 0
    ```

    because the initial queue contains oranges that are **already rotten at minute 0**.

    The first BFS level does not represent one minute of infection before it begins.

    It represents processing the initial state.

    For example:

    ```text
    2 → 1 → 1
    ```

    Initially:

    ```text
    Minute 0:

    2 1 1
    ↑
    initial queue
    ```

    Our BFS processes the initial rotten orange first.

    If:

    ```python
    time = -1
    ```

    then after processing the initial level:

    ```python
    time += 1
    ```

    gives:

    ```text
    time = 0
    ```

    Now the first newly rotten orange is in the queue.

    After processing that level:

    ```text
    time = 1
    ```

    Then after processing the next level:

    ```text
    time = 2
    ```

    So the time correctly corresponds to the number of minutes needed.

    Consider the simplest case:

    ```text
    grid = [[2]]
    ```

    There are no fresh oranges.

    The correct answer is:

    ```text
    0
    ```

    But if we initialize:

    ```python
    time = 0
    ```

    then the initial rotten orange is still in the queue.

    The BFS processes it and executes:

    ```python
    time += 1
    ```

    giving:

    ```text
    1
    ```

    which is incorrect.

    By starting from:

    ```python
    time = -1
    ```

    we get:

    ```text
    -1 + 1 = 0
    ```

    which correctly represents:

    ```text
    0 minutes needed
    ```

    There is another edge case.

    Suppose:

    ```text
    grid = [[0]]
    ```

    There are:

    ```text
    no fresh oranges
    no rotten oranges
    ```

    So:

    ```python
    queue = []
    ```

    The BFS never runs.

    Therefore:

    ```text
    time = -1
    ```

    remains unchanged.

    But the correct answer should be:

    ```text
    0
    ```

    because there are no fresh oranges that need to rot.

    Therefore:

    ```python
    return max(time, 0)
    ```

    converts:

    ```text
    -1 → 0
    ```

    for this edge case.

    After BFS finishes, some fresh oranges may still remain.

    For example:

    ```text
    2 1 1
    0 1 1
    1 0 1
    ↑
    ```

    The bottom-left fresh orange is isolated.

    It can never be reached by any rotten orange.

    Therefore, after BFS, we scan the grid again:

    ```python
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                return -1
    ```

    If any:

    ```text
    1
    ```

    still exists, return:

    ```text
    -1
    ```

    Otherwise, all fresh oranges were successfully infected.

    ## Step 1

    Create the queue and find all initial rotten oranges:

    ```python
    queue = deque()

    for i in range(m):
        for j in range(n):
            if grid[i][j] == 2:
                queue.append((i, j))
    ```

    This creates a **Multi-Source BFS**.

    ## Step 2

    Process the queue level by level:

    ```python
    while queue:
        for _ in range(len(queue)):
    ```

    Each level represents one stage of the spreading process.

    ## Step 3

    For every rotten orange, check its four neighbors:

    ```python
    for move in dirs:
    ```

    If the neighbor is a fresh orange:

    ```python
    grid[nextNode[0]][nextNode[1]] == 1
    ```

    mark it rotten:

    ```python
    grid[nextNode[0]][nextNode[1]] = 2
    ```

    and add it to the queue:

    ```python
    queue.append(nextNode)
    ```

    ## Step 4

    After finishing one BFS level:

    ```python
    time += 1
    ```

    ## Step 5

    After BFS, check whether any fresh orange remains:

    ```python
    if grid[i][j] == 1:
        return -1
    ```

    Otherwise:

    ```python
    return max(time, 0)
    ```

    Let:

    ```text
    m = number of rows
    n = number of columns
    ```

    Each cell can enter the queue at most once.

    Therefore, BFS takes:

    ```text
    O(m × n)
    ```

    The initial and final scans also take:

    ```text
    O(m × n)
    ```

    So the total time complexity is:

    ```text
    O(m × n)
    ```

    ## Space Complexity

    In the worst case, the queue may contain:

    ```text
    O(m × n)
    ```

    cells.

    Therefore:

    ```text
    O(m × n)
    ```



    This problem is a classic:

    ```text
    Multi-Source BFS
    +
    Level-Order BFS
    ```

    ### Multi-Source BFS

    All initially rotten oranges are BFS starting points:

    ```python
    if grid[i][j] == 2:
        queue.append((i, j))
    ```

    ### One BFS Level = One Stage of Time

    Use:

    ```python
    for _ in range(len(queue)):
    ```

    to process exactly the nodes belonging to the current level.

    Newly rotten oranges are added to the queue but processed in the next level.

    ### No Separate `visited`

    When a fresh orange becomes rotten:

    ```python
    grid[r][c] = 2
    ```

    it is already marked as visited.

    ### `time = -1`

    The initial rotten oranges already exist at:

    ```text
    minute 0
    ```

    so the initial BFS level should not count as an additional minute.

    Therefore:

    ```python
    time = -1
    ```

    ### Check Remaining Fresh Oranges

    After BFS:

    ```python
    if grid[i][j] == 1:
        return -1
    ```

    means some fresh orange is unreachable.

    The core pattern to remember is:

    ```text
    Put ALL sources into queue
            ↓
    Process BFS level
            ↓
    Spread to valid neighbors
            ↓
    Mark neighbors visited immediately
            ↓
    time += 1
            ↓
    Process next level
    ```

    This is the standard pattern for problems where:

    ```text
    multiple sources spread simultaneously over time
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 21, 2026 |

## Python solution

```python
from collections import deque

class Solution:
    def orangesRotting(self, grid: list[list[int]]) -> int:
        m, n = len(grid), len(grid[0])
        dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        queue = deque()
        time = -1

        # Add all initially rotten oranges
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    queue.append((i, j))

        # Multi-Source + Level-Order BFS
        while queue:
            for _ in range(len(queue)):
                curNode = queue.popleft()

                for move in dirs:
                    nextNode = (
                        curNode[0] + move[0],
                        curNode[1] + move[1]
                    )

                    if (
                        0 <= nextNode[0] < m
                        and 0 <= nextNode[1] < n
                        and grid[nextNode[0]][nextNode[1]] == 1
                    ):
                        grid[nextNode[0]][nextNode[1]] = 2
                        queue.append(nextNode)

            time += 1

        # Check whether any fresh orange is unreachable
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    return -1

        return max(time, 0)
```

---

[Back to Bonus Graph](index.md) · [All Algorithm notes](../index.md)
