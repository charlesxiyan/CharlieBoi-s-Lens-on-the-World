---
hide:
  - toc
---

# 986. Interval List Intersections

*Edited by Charles*

**Medium · Python · Intervals · Two pointers**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/interval-list-intersections/description/).

    Two lists each contain sorted, pairwise disjoint closed intervals. Return all intersections between the lists. A single shared endpoint counts as an intersection.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    The intersection starts at the larger start and ends at the smaller end. After checking a pair, discard the interval that ends first: it cannot intersect a future interval in the other list, because that future interval starts after the other current interval ends.

    ## Algorithm Steps

    1. Start pointers `i` and `j` at zero.
    2. Compute `start = max(...)` and `end = min(...)`.
    3. Append `[start, end]` if `start <= end`.
    4. Advance the pointer whose interval ends earlier; on a tie, this code advances `j`.
    5. Stop when either list is exhausted.

    ## Hand Simulation

    For `firstList = [[1,4],[7,9]]` and `secondList = [[2,6],[9,11]]`:

    | Current pair | Intersection | Advance |
    | --- | --- | --- |
    | `[1,4]`, `[2,6]` | `[2,4]` | `i` |
    | `[7,9]`, `[2,6]` | None | `j` |
    | `[7,9]`, `[9,11]` | `[9,9]` | `i` |

    The result is `[[2,4],[9,9]]`.

    ## Important Implementation Details

    Use `<=`, not `<`: these intervals are closed, so `[9,9]` is valid. Equal endpoints allow either pointer to advance; advancing only `j` may cause one extra comparison but cannot lose an intersection. The sorted, disjoint-within-each-list assumptions justify pointer movement. Empty lists return an empty result.

    ## Time and Space Complexity

    For list lengths $M,N$, time is $O(M+N)$ and auxiliary space excluding output is $O(1)$. If $K$ intersections are returned, output storage is $O(K)$.

    ## Key Takeaways

    Interval endpoint conventions determine the comparison operator; sorted order determines which pointer can safely move.

## Python solution

```python
class Solution:
    def intervalIntersection(self, firstList: list[list[int]], secondList: list[list[int]]) -> list[list[int]]:
        res = []
        l1, l2 = len(firstList), len(secondList)

        i, j = 0, 0

        while i < l1 and j < l2:
            start = max(firstList[i][0], secondList[j][0])
            end = min(firstList[i][1], secondList[j][1])

            if start <= end:
                res.append([start, end])

            if firstList[i][1] < secondList[j][1]:
                i += 1
            else:
                j += 1

        return res
```

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
