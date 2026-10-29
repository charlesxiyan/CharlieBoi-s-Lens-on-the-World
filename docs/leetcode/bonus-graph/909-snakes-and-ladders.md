---
hide:
  - toc
---

# 909. Snakes and Ladders

*Edited by Charles*

**Medium · Python · Graph · BFS · Matrix Coordinates**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/snakes-and-ladders/description/).

    An `n × n` board is numbered from `1` at the bottom-left to `n²`, reversing
    direction on each row. Starting at `1`, choose a roll from 1 to 6 without
    overshooting `n²`. If the landing square starts a snake or ladder, take it.

    A board entry of `-1` means no jump; any other entry is a destination label.
    Take at most one jump per roll, even if its destination starts another jump.
    Find the minimum number of rolls to reach `n²`, or return `-1` if unreachable.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    !!! tip "One complete dice roll is one graph edge"
        Use the square where a turn ends as the BFS state. For each possible roll,
        apply a snake or ladder once, then enqueue that final destination.

    A square may lead to up to six next-turn states. Each transition costs
    **one roll**, regardless of how far a ladder moves us or how far a snake
    sends us backward. This is an unweighted shortest-path problem, so BFS
    visits states in increasing number of rolls.

    The question asks for the best possible sequence of rolls, not the expected
    number of rolls of a random game. We explore every legal roll rather than
    simulating random dice.

    ## Convert a Square Number to Board Coordinates

    Working with labels is easier for movement: from square `s`, try labels
    `s + 1` through `min(s + 6, n²)`. But we still need a matrix position to
    check whether each landing square has a jump.

    First make the label zero-based:

    $$
    q,\;t=\operatorname{divmod}(s-1,n).
    $$

    Here `q` is the row number **counted from the bottom**, and `t` is the
    position along that row before adjusting its direction.

    - Matrix row: `row = n - 1 - q`, because Python indexes from the top.
    - If `q` is even, labels run left to right: `col = t`.
    - If `q` is odd, labels run right to left: `col = n - 1 - t`.

    For a `3 × 3` board, the numbering is:

    ```text
    7  8  9
    6  5  4
    1  2  3
    ```

    For square `6`, `divmod(5, 3)` gives `(1, 2)`. Thus `row = 1` and
    `col = 3 - 1 - 2 = 0`, matching the middle-left cell.

    **Use the row counted from the bottom for parity.** Using the top-indexed
    matrix row directly would give the wrong orientation for some board sizes.

    ## Algorithm Steps

    1. Put `(1, 0)` in a `deque`: square 1 reached with zero rolls.
    2. Mark square 1 visited.
    3. Pop `(square, rolls)` from the front of the queue.
    4. If `square == n²`, return `rolls`.
    5. For each legal landing label within six steps:
        - Convert it to `(row, col)`.
        - If the board contains a jump, use its destination; otherwise use the landing label.
        - If that final destination is unvisited, mark it and enqueue
          `(destination, rolls + 1)`.
    6. If the queue empties without reaching the goal, return `-1`.

    ## Hand Simulation

    Consider this board:

    ```python
    board = [
        [3, -1, -1],
        [-1, -1, -1],
        [-1, 8, -1],
    ]
    ```

    The numbering diagram above shows that this represents a ladder `2 → 8`
    and a snake `7 → 3`. The goal is square 9.

    **Start:** queue `[(1, 0)]`, visited `{1}`.

    From square 1, inspect all six possible landing squares:

    | Roll | Landing square | End-of-turn destination | Action |
    | --- | --- | --- | --- |
    | 1 | 2 | 8 | Take the ladder; enqueue `(8, 1)`. |
    | 2 | 3 | 3 | Enqueue `(3, 1)`. |
    | 3 | 4 | 4 | Enqueue `(4, 1)`. |
    | 4 | 5 | 5 | Enqueue `(5, 1)`. |
    | 5 | 6 | 6 | Enqueue `(6, 1)`. |
    | 6 | 7 | 3 | Take the snake; 3 is already visited. |

    The queue is now `[(8, 1), (3, 1), (4, 1), (5, 1), (6, 1)]`.
    Popping `(8, 1)`, the only legal next landing label is 9, so enqueue `(9, 2)`.
    BFS finishes processing earlier queued states before popping `(9, 2)` and
    returning **2**.

    A shortest route is:

    ```text
    1 --roll 1--> 2 --ladder--> 8 --roll 1--> 9
       first turn                  second turn
    ```

    The ladder does not add a third turn.

    ## Important Implementation Details

    **Follow only one jump per roll.** If `2 → 8` and `8 → 4` are both present,
    landing on 2 ends that turn at 8, not 4. Read the original landing cell once;
    do not use a loop that follows jump chains.

    **Mark the final destination, not just the dice landing square.** The queue
    models positions at the start of the next turn. Different rolls can lead to
    the same final position, which only needs to be explored once.

    **Mark on enqueue.** BFS first discovers a final position using the fewest
    rolls. Scheduling it again cannot improve its answer, and snakes can otherwise
    create repeated visits or cycles.

    **Bound rolls by the goal.** There is no transition for an overshoot. The
    range's upper bound needs `+1` because Python excludes the endpoint.

    **The start requires zero rolls.** A snake or ladder reached during a turn
    is part of that same turn. Every queued successor therefore uses `rolls + 1`.

    **Do not mutate the board.** A separate visited set tracks search state;
    the board continues to describe the original jumps.

    ## Why BFS Gives the Minimum

    Queue entries with zero rolls are processed first, then entries with one
    roll, then two, and so on. Each complete move has cost one, so the first
    discovered distance to a square is minimal. When the goal is popped, no
    path with fewer rolls can still be waiting to be explored.

    If the queue empties, every reachable end-of-turn square has been processed.
    The absent goal is therefore unreachable. Dijkstra is unnecessary because
    these graph edges all have the same cost.

    ## Time and Space Complexity

    There are $n^2$ possible square labels, each with at most six outgoing moves.

    | Resource | Complexity | Reason |
    | --- | --- | --- |
    | Time | $O(n^2)$ | Each final square is enqueued at most once, with at most six constant-time transitions. |
    | Auxiliary space | $O(n^2)$ | The visited set and queue can store a linear number of board squares. |

    Coordinate conversion uses constant time. We do not need to construct an
    explicit adjacency list or flatten the whole board.

    ## Key Takeaways

    - Model a complete turn as one edge, including its forced jump.
    - Use BFS for minimum moves when each move has equal cost.
    - Separate the rolled-to label from the final destination.
    - Convert serpentine labels using the row counted from the bottom.
    - Resolve a jump once per roll and deduplicate final states.

## Python solution

```python
from collections import deque
from typing import List


class Solution:
    def snakesAndLadders(self, board: List[List[int]]) -> int:
        n = len(board)
        goal = n * n

        def coordinates(square: int):
            row_from_bottom, offset = divmod(square - 1, n)
            row = n - 1 - row_from_bottom
            col = offset if row_from_bottom % 2 == 0 else n - 1 - offset
            return row, col

        queue = deque([(1, 0)])
        visited = {1}

        while queue:
            square, rolls = queue.popleft()
            if square == goal:
                return rolls

            for landing in range(square + 1, min(square + 6, goal) + 1):
                row, col = coordinates(landing)
                jump = board[row][col]
                destination = landing if jump == -1 else jump

                if destination not in visited:
                    visited.add(destination)
                    queue.append((destination, rolls + 1))

        return -1
```

---

[Back to Bonus Graph](index.md) · [All Algorithm notes](../index.md)
