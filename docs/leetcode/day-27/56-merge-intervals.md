---
hide:
  - toc
---

# 56. Merge Intervals

*Edited by Charles*

**Medium · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/merge-intervals/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Recall [452. Minimum Number of Arrows to Burst Balloons](../day-26/452-minimum-number-of-arrows-to-burst-balloons.md) and [435. Non-overlapping Intervals](../day-26/435-non-overlapping-intervals.md) for the core idea of the following solution. The sorting in the solution is based on the head of each interval, i.e. an ascending order by heads.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [452. Minimum Number of Arrows to Burst Balloons](../day-26/452-minimum-number-of-arrows-to-burst-balloons.md) and [435. Non-overlapping Intervals](../day-26/435-non-overlapping-intervals.md) for the core idea of the following solution.
    2. The sorting in the solution is based on the head of each interval, i.e. an ascending order by heads.
    3. **Attention**: the endpoints of the merged intervals are updated by parsing the interval (a list) to an object, which actually share the same address as the merged interval. Thus, any adjustment on the object is also reflected on the interval. The reason why it works is data structures like a list, a dictionary, etc are mutable.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 30, 2026 |

## Python solution

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        n = len(intervals)
        intervals.sort(key = lambda x: x[0])

        result = [intervals[0]]

        for i in range(1, n):
            cur = intervals[i]
            pre = result[-1]

            if cur[0] <= pre[1]:
                pre[1] = max(pre[1], cur[1])

            else:
                result.append(cur)

        return result
```

---

[Back to Day 27 Greedy Algorithm V](index.md) · [All Algorithm notes](../index.md)
