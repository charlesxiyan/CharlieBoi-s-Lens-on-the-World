---
hide:
  - toc
---

# 253. Meeting Rooms II

*Edited by Charles*

**Medium · Python · Hash table · Sweep line · Difference events**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/meeting-rooms-ii/description/).

    Find the minimum rooms needed to hold all meetings. Each meeting occupies `[start, end)`, so a room can be reused at the exact time its previous meeting ends.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    A room is needed for every simultaneously active meeting. Store `+1` at each start and `-1` at each end; the prefix sum in chronological order gives the active count. Its maximum is both a lower bound on rooms and sufficient capacity, since a new meeting can reuse a room whenever fewer meetings are active.

    ## Algorithm Steps

    1. Accumulate start and end changes in `record`.
    2. Sort its distinct timestamps.
    3. Add each timestamp’s net change to `curr`.
    4. Track the largest active count in `res`.

    ## Hand Simulation

    For `[[0,8],[2,5],[5,9]]`:

    | Time | Net change | Active after change |
    | --- | --- | --- |
    | 0 | +1 | 1 |
    | 2 | +1 | 2 |
    | 5 | 0 | 2 |
    | 8 | −1 | 1 |
    | 9 | −1 | 0 |

    At time 5, one meeting ends and another begins. Two rooms suffice; there is no instantaneous need for three.

    ## Important Implementation Details

    A dictionary combines all changes at the same timestamp before the maximum is checked. That automatically respects half-open intervals. The dictionary itself is not chronological, so `sorted(record)` is essential. Empty input naturally returns `0`. This counts capacity, rather than assigning room IDs.

    ## Time and Space Complexity

    For $M$ meetings and $U\leq 2M$ distinct endpoints, expected time is $O(M+U\log(U+1))$ and space is $O(U)$.

    ## Key Takeaways

    For overlap counts, record changes at endpoints instead of visiting every time value.

## Python solution

```python
from collections import defaultdict

class Solution:
    def minMeetingRooms(self, intervals: list[list[int]]) -> int:
        record = defaultdict(int)

        for start, end in intervals:
            record[start] += 1
            record[end] -= 1

        curr = 0
        res = 0

        for time in sorted(record):
            curr += record[time]
            res = max(res, curr)

        return res
```

---

[Back to Bonus Hash Table](index.md) · [All Algorithm notes](../index.md)
