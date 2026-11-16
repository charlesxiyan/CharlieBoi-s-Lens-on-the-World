---
hide:
  - toc
---

# 149. Max Points on a Line

*Edited by Charles*

**Hard · Python · Math · Geometry · Hash map**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/max-points-on-a-line/description/).

    Given distinct integer-coordinate points, find the largest number lying on one straight line. The official problem bounds each coordinate between -10,000 and 10,000.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Fix an anchor point. Other points are on the same line through this anchor exactly when they have the same slope. Count these slopes in a fresh dictionary for each anchor, then include the anchor itself.

    ## Algorithm Steps

    1. Set `res = 1` for the nonempty input.
    2. For each anchor `i`, create a new slope-count dictionary.
    3. Compare it with each later point `j`, using infinity for vertical lines and `dy / dx` otherwise.
    4. Update the answer with `record[slope] + 1`.

    ## Hand Simulation

    For `[[0,0],[1,1],[2,2],[0,2]]`, anchor `[0,0]` sees slopes `1`, `1`, and infinity. The slope-1 count is `2`; adding the anchor gives `3`. A vertical pair contributes only `2`. The final answer is `3`.

    ## Important Implementation Details

    Only later points are necessary: on any best line, its earliest-indexed point sees all the others. Reset the dictionary per anchor; equal slopes from different anchors may describe parallel, distinct lines. Distinct input points avoid the undefined duplicate-point case. `float("inf")` groups all vertical directions; positive and negative zero compare equal for horizontal lines.

    **About floating-point slopes:** this preserves Charles’s supplied implementation. Within the official small integer bounds, unequal finite rational slopes are separated by at least $1/(20000^2)$; their separation comfortably exceeds double-precision rounding at the relevant magnitudes. For arbitrary large coordinates, floats can merge distinct slopes. An exact general-purpose alternative uses a sign-normalized `(dy/gcd, dx/gcd)` pair, but that is not substituted into the code here.

    ## Time and Space Complexity

    For $N$ points, expected time is $O(N^2)$ with average constant-time dictionary access. Auxiliary space is $O(N)$ for one anchor’s slope counts.

    ## Key Takeaways

    An anchor turns a geometric line problem into frequency counting; remember to count the anchor and distinguish vertical lines.

## Python solution

```python
from collections import defaultdict
class Solution:
    def maxPoints(self, points: list[list[int]]) -> int:
        n = len(points)
        res = 1
        for i in range(n):
            record = defaultdict(int)
            x1, y1 = points[i]
            for j in range(i + 1, n):
                x2, y2 = points[j]
                dx = x2 - x1
                dy = y2 - y1
                if dx == 0:
                    slope = float('inf')
                else:
                    slope = dy / dx
                record[slope] += 1
                res = max(res, record[slope] + 1)
        return res
```

---

[Back to Day 60](index.md) · [All Algorithm notes](../index.md)
