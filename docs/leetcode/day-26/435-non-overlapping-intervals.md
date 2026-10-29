---
hide:
  - toc
---

# 435. Non-overlapping Intervals

*Edited by Charles*

**Medium · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/non-overlapping-intervals/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Recall [452. Minimum Number of Arrows to Burst Balloons](452-minimum-number-of-arrows-to-burst-balloons.md) to count the non-overlapping intervals. It should be noted that, in this question, the interval with start equal to some interval’s end is not counted as an overlapped interval.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [452. Minimum Number of Arrows to Burst Balloons](452-minimum-number-of-arrows-to-burst-balloons.md) to count the non-overlapping intervals.
    2. It should be noted that, in this question, the interval with start equal to some interval’s end is not counted as an overlapped interval.
    3. Thus, the outcome of the solution is #intervals - #non-overlapping intervals.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 29, 2026 |

## Python solution

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        n = len(intervals)
        intervals.sort(key = lambda x: x[1])
        count = 1
        end = intervals[0][1]

        for interval in intervals:
            if interval[0] >= end:
                end = interval[1]
                count += 1

        return n - count
```

---

[Back to Day 26 Greedy Algorithm IV](index.md) · [All Algorithm notes](../index.md)
