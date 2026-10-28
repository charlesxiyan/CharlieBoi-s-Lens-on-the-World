---
hide:
  - toc
---

# Knight Shortest Path (A Star)

*Edited by Charles*

**Medium · Python · Graph**

!!! tip "Core idea"

    You are given a chessboard of size `1000 x 1000`.

    The coordinates of the chessboard range from `1` to `1000` on both the `x`-axis and the `y`-axis.

## Python solution

```python
import heapq

class Solution:
    def minKnightMoves(
        self,
        startX: int,
        startY: int,
        targetX: int,
        targetY: int
    ) -> int:

        moves = [(1, 2), (-1, 2), (1, -2), (-1, -2), (2, 1), (2, -1), (-2, 1), (-2, -1)]
        queue = [(self.dist((startX, startY), (targetX, targetY)), (startX, startY))]
        minSteps = {(startX, startY): 0}

        while queue:
            d, cur = heapq.heappop(queue)

            if cur == (targetX, targetY):
                return minSteps[cur]

            for move in moves:
                nextP = (cur[0] + move[0], cur[1] + move[1])
                if 1 <= nextP[0] <= 1000 and 1 <= nextP[1] <= 1000:
                    newSteps = minSteps[cur] + 1
                    if newSteps < minSteps.get(nextP, float('inf')):
                        minSteps[nextP] = newSteps
                        heapq.heappush(queue, (self.dist(nextP, (targetX, targetY)) + newSteps, nextP))

        return -1

    def dist(self, p1, p2):
        acc = 0
        for i in range(len(p1)):
            acc += (p1[i] - p2[i]) ** 2
        return acc ** 0.5
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem Description

    You are given a chessboard of size `1000 x 1000`.

    The coordinates of the chessboard range from `1` to `1000` on both the `x`-axis and the `y`-axis.

    A knight moves according to the standard chess rule:

    ```text
    (x ± 1, y ± 2)
    (x ± 2, y ± 1)
    ```

    In other words, from position `(x, y)`, the knight has up to 8 possible moves:

    ```text
    (x + 1, y + 2)
    (x + 1, y - 2)
    (x - 1, y + 2)
    (x - 1, y - 2)

    (x + 2, y + 1)
    (x + 2, y - 1)
    (x - 2, y + 1)
    (x - 2, y - 1)
    ```

    A move is valid only if the new position remains inside the chessboard.

    Given a starting position and a target position, return the minimum number of moves required for the knight to reach the target.

    ## Function Signature

    ```python
    class Solution:
        def minKnightMoves(
            self,
            startX: int,
            startY: int,
            targetX: int,
            targetY: int
        ) -> int:
    ```

    ## Parameters

    ### `startX`

    The x-coordinate of the knight's starting position.

    ### `startY`

    The y-coordinate of the knight's starting position.

    ### `targetX`

    The x-coordinate of the target position.

    ### `targetY`

    The y-coordinate of the target position.

    The starting position is:

    ```text
    (startX, startY)
    ```

    The target position is:

    ```text
    (targetX, targetY)
    ```

    ## Return Value

    Return an integer representing the minimum number of knight moves required to travel from the starting position to the target position.

    ## Example 1

    ### Input

    ```python
    startX = 5
    startY = 2
    targetX = 5
    targetY = 4
    ```

    ### Output

    ```text
    2
    ```

    ## Example 2

    ### Input

    ```python
    startX = 1
    startY = 1
    targetX = 2
    targetY = 2
    ```

    ### Output

    ```text
    4
    ```

    ## Example 3

    ### Input

    ```python
    startX = 1
    startY = 1
    targetX = 8
    targetY = 8
    ```

    ### Output

    ```text
    6
    ```

    ## Example 4

    ### Input

    ```python
    startX = 1
    startY = 1
    targetX = 8
    targetY = 7
    ```

    ### Output

    ```text
    5
    ```

    ## Example 5

    ### Input

    ```python
    startX = 2
    startY = 1
    targetX = 3
    targetY = 3
    ```

    ### Output

    ```text
    1
    ```

    ## Example 6

    ### Input

    ```python
    startX = 4
    startY = 6
    targetX = 4
    targetY = 6
    ```

    ### Output

    ```text
    0
    ```

    ## Constraints

    - `1 <= startX, startY <= 1000`
    - `1 <= targetX, targetY <= 1000`
    - The chessboard size is fixed at `1000 x 1000`.
    - The knight may only move according to standard chess knight movement.
    - Every move must stay inside the chessboard.

    ## Starter Code

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 3, 2026 |

---

[Back to Day 55 Graph XIII](index.md) · [All LeetCode notes](../index.md)
