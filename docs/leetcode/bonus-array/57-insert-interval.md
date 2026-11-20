---
hide:
  - toc
---

# 57. Insert Interval

*Edited by Charles*

**Medium · Python · Intervals · Merging**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/insert-interval/description/).

    Insert a new closed interval into a sorted list of non-overlapping intervals, merging overlaps so the resulting list remains sorted and non-overlapping.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Split the scan into three regions: intervals strictly before the new interval, intervals that overlap it, and intervals strictly after it. While merging, expand `newInterval` so its boundaries always describe the entire merged region.

    ## Algorithm Steps

    1. Append intervals ending strictly before the new interval starts.
    2. While the next interval starts at or before the expanding new end, merge its endpoints into `newInterval`.
    3. Append the merged interval once.
    4. Append all remaining intervals unchanged.

    ## Hand Simulation

    For `[[1,2],[4,6],[8,10]]` and new interval `[2,8]`, nothing ends strictly before 2. Merge `[1,2]` to get `[1,8]`, then `[4,6]` leaves it unchanged, then `[8,10]` expands it to `[1,10]`. Return `[[1,10]]`.

    ## Important Implementation Details

    After the first loop, every unprocessed interval ends at or after the new start. Therefore only `intervals[idx][0] <= newInterval[1]` is still needed to establish overlap. Requiring `newInterval[0] <= intervals[idx][0]` would incorrectly reject an interval that begins earlier but overlaps or contains the new one, such as `[1,5]` with `[2,4]`. Closed intervals merge when endpoints touch. The code mutates the supplied `newInterval` list, and the result reuses existing interval lists.

    ## Time and Space Complexity

    For $N$ existing intervals, time is $O(N)$ and auxiliary working space is $O(1)$ excluding the $O(N)$ output list.

    ## Key Takeaways

    Use what earlier scan phases have already established; a redundant-looking lower-bound test can actually be wrong.

## Python solution

```python
class Solution:
    def insert(self, intervals: list[list[int]], newInterval: list[int]) -> list[list[int]]:
        res = []
        l = len(intervals)
        idx = 0

        while idx < l and intervals[idx][1] < newInterval[0]:
            res.append(intervals[idx])
            idx += 1

        while idx < l and intervals[idx][0] <= newInterval[1]:
            # Case 1: [1, 3], newInterval = [2, 5]
            # Case 2: [1, 5], newInterval = [2, 4]
            # Therefore, do not require newInterval[0] <= intervals[idx][0] <= newInterval[1].
            newInterval[0] = min(newInterval[0], intervals[idx][0])
            newInterval[1] = max(newInterval[1], intervals[idx][1])
            idx += 1

        res.append(newInterval)

        while idx < l:
            res.append(intervals[idx])
            idx += 1

        return res
```

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
