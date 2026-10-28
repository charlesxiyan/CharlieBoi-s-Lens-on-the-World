---
hide:
  - toc
---

# 787. Cheapest Flights Within K Stops (Bellman-Ford, Single-Source Shortest Path with a Limited Number of Edges)

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)

!!! tip "Core idea"

    This problem can be solved using a modified **Bellman-Ford algorithm**.

    The key difference from standard Bellman-Ford is that we are only allowed to use at most `k` stops.

## Python solution

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        INF = float('inf')
        dist = [INF] * n
        dist[src] = 0

        for _ in range(k + 1):
            new = dist[:]
            for u, v, w in flights:
                if dist[u] + w < new[v]:
                    new[v] = dist[u] + w
            dist = new

        return dist[dst] if dist[dst] != INF else -1
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Approach: Bellman-Ford with Limited Relaxations

    This problem can be solved using a modified **Bellman-Ford algorithm**.

    The key difference from standard Bellman-Ford is that we are only allowed to use at most `k` stops.

    If a path contains `k` intermediate stops, then it contains at most:

    ```text
    k + 1 edges
    ```

    Therefore, instead of performing `n - 1` rounds of relaxation, we only perform:

    ```python
    k + 1
    ```

    rounds.

    ## Store All Edges

    We first store all flights as edges:

    ```python
    edges = []

    for u, v, w in flights:
        edges.append((u, v, w))
    ```

    Each edge represents:

    ```text
    (source, destination, cost)
    ```

    Since Bellman-Ford directly iterates through all edges, we do not need to build an adjacency list.

    ## Define `minCost`

    ```python
    minCost = [float('inf')] * n
    minCost[src] = 0
    ```

    `minCost[i]` represents:

    > The minimum cost to reach city `i` from `src` using at most the number of edges allowed by the current relaxation round.
    >

    Initially:

    ```text
    src          → 0
    other cities → infinity
    ```

    because we have not used any flights yet.

    ## Why `k + 1` Relaxations?

    The problem allows at most `k` stops.

    For example:

    ```text
    src → A → B → dst
    ```

    There are:

    ```text
    2 stops: A, B
    3 edges
    ```

    Therefore:

    ```text
    k stops = k + 1 edges
    ```

    Bellman-Ford has an important property:

    ```text
    After 1 round → shortest paths using at most 1 edge
    After 2 rounds → shortest paths using at most 2 edges
    After 3 rounds → shortest paths using at most 3 edges
    ...
    ```

    Therefore, to allow at most `k` stops, we perform:

    ```python
    for _ in range(k + 1):
    ```

    This is the most important part of this problem.

    At the beginning of every round:

    ```python
    temp = minCost.copy()
    ```

    We read values from:

    ```python
    minCost
    ```

    but write new values into:

    ```python
    temp
    ```

    During relaxation:

    ```python
    if minCost[source] + cost < temp[destination]:
        temp[destination] = minCost[source] + cost
    ```

    At the end of the round:

    ```python
    minCost = temp
    ```

    The purpose of `temp` is to make sure that **each Bellman-Ford round extends the path by at most one edge**.

    ## What Goes Wrong Without `temp`?

    Consider:

    ```text
    0 --100--> 1 --100--> 2 --100--> 3
    ```

    Initially:

    ```text
    minCost = [0, inf, inf, inf]
    ```

    Suppose we directly update `minCost` in place.

    ### Process `0 → 1`

    ```text
    minCost[1] = 100

    [0, 100, inf, inf]
    ```

    ### Process `1 → 2`

    Because `minCost[1]` was just updated in the **same round**, we can immediately use it:

    ```text
    minCost[2] = 200

    [0, 100, 200, inf]
    ```

    ### Process `2 → 3`

    Again, `minCost[2]` was just updated:

    ```text
    minCost[3] = 300

    [0, 100, 200, 300]
    ```

    This means that in **one relaxation round**, we traveled:

    ```text
    0 → 1 → 2 → 3
    ```

    which uses **3 edges**.

    This destroys the meaning of:

    ```text
    1 round = at most 1 additional edge
    ```

    and can violate the `k`-stop constraint.

    Now consider the same graph:

    ```text
    0 --100--> 1 --100--> 2 --100--> 3
    ```

    Initially:

    ```text
    minCost = [0, inf, inf, inf]
    ```

    At the beginning of the first round:

    ```python
    temp = minCost.copy()
    ```

    so:

    ```text
    minCost = [0, inf, inf, inf]
    temp    = [0, inf, inf, inf]
    ```

    ## Round 1

    Process:

    ```text
    0 → 1
    ```

    We update:

    ```text
    temp[1] = 100
    ```

    so:

    ```text
    minCost = [0, inf, inf, inf]
    temp    = [0, 100, inf, inf]
    ```

    Now process:

    ```text
    1 → 2
    ```

    The important point is that we check:

    ```python
    minCost[1]
    ```

    NOT:

    ```python
    temp[1]
    ```

    Since:

    ```text
    minCost[1] = inf
    ```

    we cannot use the newly discovered path to node `1` in the same round.

    Therefore, node `2` is not updated.

    After the round:

    ```python
    minCost = temp
    ```

    Now:

    ```text
    minCost = [0, 100, inf, inf]
    ```

    This represents paths using at most **1 edge**.

    ## Round 2

    Create another copy:

    ```text
    minCost = [0, 100, inf, inf]
    temp    = [0, 100, inf, inf]
    ```

    Now:

    ```text
    1 → 2
    ```

    can use:

    ```text
    minCost[1] = 100
    ```

    so:

    ```text
    temp[2] = 200
    ```

    After this round:

    ```text
    minCost = [0, 100, 200, inf]
    ```

    This represents paths using at most **2 edges**.

    ## Round 3

    Similarly:

    ```text
    minCost = [0, 100, 200, inf]
    ```

    allows:

    ```text
    2 → 3
    ```

    so:

    ```text
    minCost = [0, 100, 200, 300]
    ```

    This represents paths using at most **3 edges**.

    Therefore:

    ```text
    Round 1 → at most 1 edge
    Round 2 → at most 2 edges
    Round 3 → at most 3 edges
    ...
    Round k+1 → at most k+1 edges
    ```

    This is exactly what we need.

    ## Read from `minCost`, Write to `temp`

    The key relaxation is:

    ```python
    if minCost[source] != float('inf') and \
       minCost[source] + cost < temp[destination]:

        temp[destination] = minCost[source] + cost
    ```

    Notice the difference:

    ```text
    Read source distance:        minCost[source]
    Write destination distance:  temp[destination]
    ```

    This is intentional.

    `minCost` represents the results from the **previous round**.

    `temp` represents the results for the **current round**.

    Therefore:

    ```text
    Previous round
        minCost
           ↓
    use one more edge
           ↓
         temp
           ↓
    end of round
           ↓
    minCost = temp
    ```

    This guarantees that one round can add at most **one new edge** to a path.

    ## Why `temp = minCost.copy()` Instead of a New `inf` Array?

    We use:

    ```python
    temp = minCost.copy()
    ```

    instead of:

    ```python
    temp = [float('inf')] * n
    ```

    because the problem asks for paths using **at most** `k + 1` edges, not exactly `k + 1` edges.

    For example, if we already know:

    ```text
    src → A
    cost = 100
    ```

    after one round, that result should still be valid after the second round.

    Copying:

    ```python
    temp = minCost.copy()
    ```

    preserves all previously discovered cheaper paths while also allowing paths using one additional edge to be discovered.

    Therefore:

    ```text
    Round i stores the minimum cost using at most i edges.
    ```

    ## Early Termination

    We use:

    ```python
    updated = False
    ```

    and whenever a better path is found:

    ```python
    updated = True
    ```

    After scanning all edges:

    ```python
    if not updated:
        break
    ```

    If no value changes during a complete relaxation round, further rounds cannot improve any shortest path.

    Therefore, we can terminate early.

    ## Complexity

    Let:

    ```text
    V = number of cities
    E = number of flights
    ```

    We perform at most:

    ```text
    k + 1
    ```

    rounds.

    Each round scans all `E` edges.

    Therefore:

    ```text
    Time: O((k + 1) * E)
    ```

    The `minCost` and `temp` arrays require:

    ```text
    O(V)
    ```

    space.

    The `edges` list requires:

    ```text
    O(E)
    ```

    space.

    Therefore:

    ```text
    Space: O(V + E)
    ```

    Since `flights` itself already contains all edges, the extra algorithmic distance-array space is `O(V)`.

    ## Code

    ```python
    class Solution:
        def findCheapestPrice(
            self,
            n: int,
            flights: List[List[int]],
            src: int,
            dst: int,
            k: int
        ) -> int:

            edges = []

            for u, v, w in flights:
                edges.append((u, v, w))

            minCost = [float('inf')] * n
            minCost[src] = 0

            for _ in range(k + 1):
                updated = False
                temp = minCost.copy()

                for source, destination, cost in edges:
                    if (
                        minCost[source] != float('inf')
                        and minCost[source] + cost < temp[destination]
                    ):
                        temp[destination] = minCost[source] + cost
                        updated = True

                if not updated:
                    break

                minCost = temp

            return -1 if minCost[dst] == float('inf') else minCost[dst]
    ```

    ## Key Takeaway

    The most important idea in this problem is:

    > `temp` prevents updates from the current relaxation round from being used again in the same round.
    >

    Without `temp`:

    ```text
    One round may use multiple edges
    → cannot enforce the k-stop constraint
    ```

    With `temp`:

    ```text
    Read from minCost
           ↓
    Use exactly one additional edge
           ↓
    Write to temp
           ↓
    minCost = temp
    ```

    Therefore:

    ```text
    1 round     → at most 1 edge
    2 rounds    → at most 2 edges
    ...
    k + 1 rounds → at most k + 1 edges
    ```

    Since:

    ```text
    k stops = k + 1 edges
    ```

    performing `k + 1` Bellman-Ford relaxation rounds correctly enforces the constraint.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 31, 2026 |

---

[Back to Day 53 Graph XI](index.md) · [All LeetCode notes](../index.md)
