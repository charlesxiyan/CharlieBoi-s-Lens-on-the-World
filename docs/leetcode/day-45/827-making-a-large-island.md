---
hide:
  - toc
---

# 827. Making a Large Island

*Edited by Charles*

**Hard · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/making-a-large-island/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        You are given an `n x n` binary grid where:

        You may change **at most one `0` into `1`**.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    You are given an `n x n` binary grid where:

    ```text
    1 = land
    0 = water
    ```

    You may change **at most one `0` into `1`**.

    Return the size of the largest island that can be created.

    The key idea is:

    > First, identify every existing island and record its area.
    >
    >
    > Then, for each `0`, check which neighboring islands would be connected if this `0` were changed into `1`.
    >

    This problem is an extension of **LeetCode 695. Max Area of Island**.

    ## Step 1: Label Every Island

    Instead of only marking visited land as `0`, assign each island a unique ID.

    For example:

    ```text
    Original grid

    1 1 0
    1 0 1
    0 1 1
    ```

    After labeling:

    ```text
    2 2 0
    2 0 3
    0 3 3
    ```

    Here:

    ```text
    Island 2 → area = 3
    Island 3 → area = 3
    ```

    We can store the areas in a dictionary:

    ```python
    area = {
        2: 3,
        3: 3
    }
    ```

    We start island IDs from `2` because:

    ```text
    0 = water
    1 = original land
    2, 3, 4, ... = island IDs
    ```

    ## Step 2: DFS to Calculate Island Area

    For every unvisited land cell:

    ```python
    grid[i][j] == 1
    ```

    run DFS.

    During DFS:

    - change the current cell from `1` to the island ID,
    - recursively visit all connected land cells,
    - return the total island area.

    The DFS can look like:

    ```python
    def dfs(i, j, island_id):
        if i < 0 or i >= n or j < 0 or j >= n or grid[i][j] != 1:
            return 0

        grid[i][j] = island_id

        return (
            1
            + dfs(i - 1, j, island_id)
            + dfs(i + 1, j, island_id)
            + dfs(i, j - 1, island_id)
            + dfs(i, j + 1, island_id)
        )
    ```

    Each island gets a unique label and its area is stored.

    ## Step 3: Try Changing Every `0` Into `1`

    Now traverse the grid again.

    For every water cell:

    ```python
    grid[i][j] == 0
    ```

    imagine changing it into land.

    The new island area starts with:

    ```text
    1
    ```

    for the converted cell itself.

    Then check its four neighbors:

    ```text
    Up
    Down
    Left
    Right
    ```

    If a neighboring cell belongs to an island, add that island's area.

    ## Why Do We Need a Set?

    Suppose the grid is:

    ```text
    2 2
    2 0
    ```

    The `0` touches island `2` from both the top and the left.

    If we simply add the area for every neighbor:

    ```text
    top  → island 2
    left → island 2
    ```

    we would count the same island twice.

    Therefore, we must collect **unique island IDs** first.

    For example:

    ```python
    neighbors = set()
    ```

    Then:

    ```python
    neighbors.add(grid[x][y])
    ```

    The new area is:

    ```python
    new_area = 1

    for island_id in neighbors:
        new_area += area[island_id]
    ```

    This prevents double counting.

    ## Hand Simulation

    Consider:

    ```python
    grid = [
        [1,0],
        [0,1]
    ]
    ```

    Initially:

    ```text
    1 0
    0 1
    ```

    ### Step 1: Label Islands

    The top-left island gets ID `2`:

    ```text
    2 0
    0 1
    ```

    Its area is:

    ```text
    area[2] = 1
    ```

    The bottom-right island gets ID `3`:

    ```text
    2 0
    0 3
    ```

    Its area is:

    ```text
    area[3] = 1
    ```

    So:

    ```python
    area = {
        2: 1,
        3: 1
    }
    ```

    ### Step 2: Try the Top-Right `0`

    Consider changing:

    ```text
    (0,1)
    ```

    into `1`.

    Its neighbors include:

    ```text
    left  → island 2
    down  → island 3
    ```

    So the connected island IDs are:

    ```python
    {2, 3}
    ```

    New area:

    ```text
    1 + area[2] + area[3]
    = 1 + 1 + 1
    = 3
    ```

    ### Step 3: Try the Bottom-Left `0`

    The same thing happens.

    It also connects islands `2` and `3`.

    New area:

    ```text
    3
    ```

    Therefore:

    ```python
    Result = 3
    ```

    ## Larger Example

    Consider:

    ```python
    grid = [
        [1,1,0],
        [1,0,1],
        [0,1,1]
    ]
    ```

    After labeling islands:

    ```text
    2 2 0
    2 0 3
    0 3 3
    ```

    Areas:

    ```text
    Island 2 = 3
    Island 3 = 3
    ```

    Now consider the center `0`:

    ```text
    2 2 0
    2 X 3
    0 3 3
    ```

    It touches:

    ```text
    Island 2
    Island 3
    ```

    So changing it into land creates:

    ```text
    1 + 3 + 3 = 7
    ```

    Therefore, the largest possible island has area:

    ```python
    7
    ```

    ## What If the Grid Has No `0`?

    For example:

    ```text
    1 1
    1 1
    ```

    There is already one island with area:

    ```text
    4
    ```

    Since we may change **at most one** `0`, we do not have to change anything.

    Therefore, the answer should be:

    ```python
    4
    ```

    This is why we should also keep track of the largest existing island area.

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n²)
    ```

    The grid is traversed a constant number of times.

    DFS visits each land cell once.

    For every `0`, we only inspect four neighbors.

    **Space Complexity**

    ```text
    O(n²)
    ```

    In the worst case, the DFS recursion stack can contain `O(n²)` cells.

    The area dictionary also stores information for all islands.

    ## Summary

    ```text
    1. Use DFS to find every island.
    2. Give each island a unique ID.
    3. Store each island's area in a dictionary.
    4. Traverse every 0.
    5. Pretend to change the 0 into 1.
    6. Collect the unique neighboring island IDs.
    7. Add their areas together plus 1.
    8. Take the maximum.
    ```

    The most important ideas are:

    > **Label each island first, then merge neighboring islands virtually.**
    >

    and:

    > **Use a set when checking neighbors so the same island is not counted twice.**
    >

    ## Common Mistakes

    - Do not simply mark visited land as `0`. We need to preserve the identity of each island so that neighboring `0`s know which islands they touch.
    - Use different IDs such as:

    ```text
    2, 3, 4, ...
    ```

    for different islands.

    - Use a `set` when collecting neighboring island IDs. The same island may touch a `0` from multiple directions.
    - Remember to count the converted water cell itself:

    ```text
    new_area = 1
    ```

    - Do not forget the case where the entire grid is already land. In that case, the answer is simply:

    ```text
    n × n
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 19, 2026 |

## Python solution

```python
class Solution:
    def largestIsland(self, grid: List[List[int]]) -> int:
        m = len(grid)
        islands = {}
        res = 0
        island_id = 2

        def dfs(i, j, island_id):
            if i < 0 or i >= m or j < 0 or j >= m or grid[i][j] != 1:
                return 0
            grid[i][j] = island_id
            return 1 + dfs(i - 1, j, island_id) + dfs(i + 1, j, island_id) + dfs(i, j - 1, island_id) + dfs(i, j + 1, island_id)

        for i in range(m):
            for j in range(m):
                if grid[i][j] == 1:
                    area = dfs(i, j, island_id)
                    islands[island_id] = area
                    res = max(res, area)
                    island_id += 1

        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

        for i in range(m):
            for j in range(m):
                if grid[i][j] == 0:
                    neighbors = set()
                    temp_area = 1
                    for p, q in directions:
                        if 0 <= i + p < m and 0 <= j + q < m and grid[i + p][j + q] > 1:
                            neighbors.add(grid[i + p][j + q])

                    for neighbor in neighbors:
                        temp_area += islands[neighbor]

                    res = max(res, temp_area)

        return res
```

---

[Back to Day 45 Graph III](index.md) · [All Algorithm notes](../index.md)
