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

    In `getPosition(num)`, first subtract one from `num` to make it zero-based.
    Then compute `row = num // n` and `col = num % n`.

    At this point, `row` counts from the bottom:

    - If `row % 2` is true, reverse the column with `col = n - 1 - col`.
    - Otherwise, leave `col` unchanged.
    - Finally set `row = n - 1 - row` to convert to Python's top-based row index.

    For a `3 × 3` board, the numbering is:

    ```text
    7  8  9
    6  5  4
    1  2  3
    ```

    For square `6`, `num` becomes 5, so `row = 5 // 3 = 1` and
    `col = 5 % 3 = 2`. The row is odd, so the column reverses:
    `col = 3 - 1 - 2 = 0`, matching the middle-left cell.

    **Use the row counted from the bottom for parity.** Using the top-indexed
    matrix row directly would give the wrong orientation for some board sizes.

    ## Algorithm Steps

    1. Put `(1, 0)` in a `deque`: square 1 reached with zero rolls.
    2. Initialize `record = {1}` to mark the starting square visited.
    3. Pop `(cur, steps)` from the front of the queue.
    4. If `cur == n * n`, return `steps`.
    5. Try `dice` values from 1 through 6:
        - Set `nxt = cur + dice`; stop the loop if it exceeds `n * n`.
        - Convert `nxt` to `(row, col)` using `getPosition(nxt)`.
        - If the cell starts a jump, update `nxt` to `board[row][col]` once.
        - If this final `nxt` is not in `record`, record it and enqueue
          `(nxt, steps + 1)`.
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

    **Start:** `queue = deque([(1, 0)])`, `record = {1}`.

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
    loop uses `range(1, 7)` to include all six dice values. Once `nxt > n * n`,
    `break` is safe because every later dice value would overshoot too.

    **The start requires zero rolls.** A snake or ladder reached during a turn
    is part of that same turn. Every queued successor therefore uses `steps + 1`.

    **Do not mutate the board.** The separate `record` set tracks visited end-of-turn squares;
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

class Solution:
    def snakesAndLadders(self, board: list[list[int]]) -> int:
        n = len(board)
        queue = deque([(1, 0)])
        record = {1}

        def getPosition(num):
            num -= 1

            row = num // n
            col = num % n

            if row % 2:
                col = n - 1 - col

            row = n - 1 - row

            return row, col

        while queue:
            cur, steps = queue.popleft()

            if cur == n * n:
                return steps

            for dice in range(1, 7):
                nxt = cur + dice
                if nxt > n * n:
                    break

                row, col = getPosition(nxt)

                if board[row][col] != -1:
                    # 跳转nxt
                    nxt = board[row][col]

                if nxt not in record:
                    record.add(nxt)
                    queue.append((nxt, steps + 1))

        return -1
```

---

[Back to Bonus Graph](index.md) · [All Algorithm notes](../index.md)
