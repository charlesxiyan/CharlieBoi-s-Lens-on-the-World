---
hide:
  - toc
---

# 1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance (Floyd-Warshall)

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)

!!! tip "Core idea"

    See the Python implementation and detailed notes below.

## Python solution

```python
class Solution:
    def findTheCity(
        self,
        n: int,
        edges: List[List[int]],
        distanceThreshold: int
    ) -> int:

        dp = [[float('inf')] * n for _ in range(n)]

        for i in range(n):
            dp[i][i] = 0

        for u, v, w in edges:
            dp[u][v] = w
            dp[v][u] = w

        # Floyd-Warshall
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    dp[i][j] = min(
                        dp[i][k] + dp[k][j],
                        dp[i][j]
                    )

        minCount, res = float('inf'), -1

        for i in range(n):
            count = 0

            for j in range(n):
                if i != j and dp[i][j] <= distanceThreshold:
                    count += 1

            if count <= minCount:
                minCount = count
                res = i

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: Floyd-Warshall Algorithm

    ### Core Idea

    For each city, we need to know how many other cities can be reached with a shortest-path distance no greater than `distanceThreshold`.

    Therefore, we need the shortest distance between **every pair of cities**.

    This is an **all-pairs shortest path** problem, which can be solved directly using the **Floyd-Warshall algorithm**.

    ## DP Definition

    Define:

    ```text
    dp[i][j]
    ```

    as:

    > The shortest distance from city `i` to city `j`.
    >

    Initially, create an `n × n` matrix:

    ```python
    dp = [[float('inf')] * n for _ in range(n)]
    ```

    If two cities are not directly connected, their distance is initially:

    ```text
    ∞
    ```

    ## Initialization

    ### Distance from a city to itself

    The distance from every city to itself is `0`:

    ```python
    for i in range(n):
        dp[i][i] = 0
    ```

    So:

    ```text
    dp[0][0] = 0
    dp[1][1] = 0
    dp[2][2] = 0
    ...
    ```

    ### Add the edges

    For every edge:

    ```python
    [u, v, w]
    ```

    cities `u` and `v` are connected with distance `w`.

    Since the graph is **undirected**:

    ```python
    for u, v, w in edges:
        dp[u][v] = w
        dp[v][u] = w
    ```

    We must update both directions.

    ## Floyd-Warshall

    The core idea of Floyd-Warshall is:

    > For every pair of cities `i` and `j`, check whether going through another city `k` gives a shorter path.
    >

    Originally:

    ```text
    i --------------------> j
    ```

    We try:

    ```text
    i --------> k --------> j
    ```

    Therefore:

    ```text
    new distance = dp[i][k] + dp[k][j]
    ```

    If this is shorter than the current:

    ```text
    dp[i][j]
    ```

    we update it.

    The transition is:

    ```python
    dp[i][j] = min(
        dp[i][j],
        dp[i][k] + dp[k][j]
    )
    ```

    ## Why Is `k` the Outer Loop?

    The Floyd-Warshall loop must be:

    ```python
    for k in range(n):
        for i in range(n):
            for j in range(n):
    ```

    Here, `k` represents the intermediate city that we are currently allowing in our shortest paths.

    Conceptually:

    ```text
    k = 0
    → allow city 0 as an intermediate city

    k = 1
    → allow cities 0, 1 as intermediate cities

    k = 2
    → allow cities 0, 1, 2 as intermediate cities

    ...

    k = n - 1
    → allow all cities as intermediate cities
    ```

    After processing all values of `k`, `dp[i][j]` contains the shortest distance between every pair of cities.

    ## Original 3D DP Interpretation

    Floyd-Warshall can originally be understood as a 3D DP.

    Define:

    ```text
    DP[k][i][j]
    ```

    as:

    > The shortest distance from `i` to `j` when only the first `k` cities are allowed to be intermediate nodes.
    >

    For each intermediate node `k`, there are two possibilities.

    ### Case 1: Do not use `k`

    ```text
    i ----------------> j
    ```

    Keep the previous shortest distance.

    ### Case 2: Use `k`

    ```text
    i ------> k ------> j
    ```

    The distance becomes:

    ```text
    distance(i, k) + distance(k, j)
    ```

    Therefore, conceptually:

    ```text
    DP[k][i][j]
    =
    min(
        previous distance from i to j,
        distance from i to k + distance from k to j
    )
    ```

    Because each stage only depends on the previous stage, the `k` dimension can be optimized away.

    This gives the standard 2D Floyd-Warshall transition:

    ```python
    dp[i][j] = min(
        dp[i][j],
        dp[i][k] + dp[k][j]
    )
    ```

    So the 2D matrix is a **space-optimized version of the original 3D DP**.

    ## Count Reachable Cities

    After Floyd-Warshall finishes, `dp[i][j]` is the shortest distance from city `i` to city `j`.

    For each city `i`:

    ```python
    count = 0
    ```

    we check every other city `j`:

    ```python
    if i != j and dp[i][j] <= distanceThreshold:
        count += 1
    ```

    The condition:

    ```python
    i != j
    ```

    is necessary because we should not count the city itself.

    The condition:

    ```python
    dp[i][j] <= distanceThreshold
    ```

    means city `j` is reachable from city `i` within the required threshold.

    ## Find the City with the Minimum Count

    We maintain:

    ```python
    minCount = float('inf')
    res = -1
    ```

    For every city:

    ```python
    if count <= minCount:
        minCount = count
        res = i
    ```

    ### Why `<=` Instead of `<`?

    This is important.

    The problem says:

    > If multiple cities have the same minimum number of reachable cities, return the city with the greatest number.
    >

    We iterate through cities in increasing order:

    ```text
    0 → 1 → 2 → ... → n - 1
    ```

    Suppose:

    ```text
    city 2 → count = 3
    city 4 → count = 3
    ```

    When processing city `4`:

    ```python
    count <= minCount
    ```

    is still `True`.

    Therefore:

    ```python
    res = 4
    ```

    replaces:

    ```python
    res = 2
    ```

    This automatically keeps the **largest city number when there is a tie**.

    ## Example

    Suppose:

    ```text
    n = 4
    edges = [
        [0, 1, 3],
        [1, 2, 1],
        [1, 3, 4],
        [2, 3, 1]
    ]

    distanceThreshold = 4
    ```

    Initially, we only know direct distances:

    ```text
          0    1    2    3

    0     0    3    ∞    ∞
    1     3    0    1    4
    2     ∞    1    0    1
    3     ∞    4    1    0
    ```

    Floyd-Warshall discovers shorter indirect paths.

    For example:

    ```text
    0 → 1 → 2
    ```

    has distance:

    ```text
    3 + 1 = 4
    ```

    so:

    ```text
    dp[0][2] = 4
    ```

    Similarly:

    ```text
    0 → 1 → 2 → 3
    ```

    has distance:

    ```text
    3 + 1 + 1 = 5
    ```

    After Floyd-Warshall finishes, every `dp[i][j]` represents the shortest distance between `i` and `j`.

    Then we count how many cities are within `distanceThreshold` for each city and select the city with the smallest count.

    ## Complexity

    Floyd-Warshall uses three nested loops:

    ```python
    for k in range(n):
        for i in range(n):
            for j in range(n):
    ```

    Therefore:

    ```text
    Time Complexity: O(n³)
    ```

    The distance matrix contains `n × n` values:

    ```text
    Space Complexity: O(n²)
    ```

    ## Key Takeaways

    ### Floyd-Warshall is for all-pairs shortest paths

    Use Floyd-Warshall when we need:

    ```text
    shortest distance between every pair of nodes
    ```

    rather than only the shortest paths from one source.

    ### Core transition

    ```python
    dp[i][j] = min(
        dp[i][j],
        dp[i][k] + dp[k][j]
    )
    ```

    Meaning:

    ```text
    Do not go through k:
    i ----------------> j

    Go through k:
    i ------> k ------> j

    Take the shorter one.
    ```

    ### Loop order

    Always remember:

    ```python
    for k in range(n):
        for i in range(n):
            for j in range(n):
    ```

    `k` must be the outer loop because it represents the DP stage: which intermediate nodes are currently allowed.

    ### Floyd-Warshall is originally a 3D DP

    Conceptually:

    ```text
    DP[k][i][j]
    ```

    represents the shortest path from `i` to `j` when only certain nodes are allowed as intermediate nodes.

    The standard implementation compresses this into:

    ```text
    dp[i][j]
    ```

    so the space complexity decreases from:

    ```text
    O(n³) → O(n²)
    ```

    ### Tie-breaking in LeetCode 1334

    Because the problem asks for the **largest city number** when counts are equal:

    ```python
    if count <= minCount:
    ```

    rather than:

    ```python
    if count < minCount:
    ```

    This allows a later, larger city index to replace an earlier one when their reachable-city counts are equal.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 3, 2026 |

---

[Back to Day 55 Graph XIII](index.md) · [All LeetCode notes](../index.md)
