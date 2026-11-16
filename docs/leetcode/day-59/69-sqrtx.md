---
hide:
  - toc
---

# 69. Sqrt(x)

*Edited by Charles*

**Easy · Python · Math · Binary Search**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/sqrtx/description/).

    For a nonnegative integer `x`, return the integer part of its square root: the largest integer `r` satisfying `r * r <= x`.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    The predicate `mid * mid <= x` is true for an initial interval of nonnegative integers and false afterward. Binary search locates the last true value.

    ## Algorithm Steps

    1. Search the closed interval `[0, x]`.
    2. If `mid` is feasible, move `left` to `mid + 1` to look for a larger feasible answer.
    3. Otherwise move `right` to `mid - 1`.
    4. When the interval is empty, return `right`.

    ## Hand Simulation

    For `x = 8`: start `[0,8]`, try `4` (too large), then `[0,3]`, try `1` (valid), then `[2,3]`, try `2` (valid), then `[3,3]`, try `3` (too large). Finish with `left = 3`, `right = 2`; return `2`.

    ## Important Implementation Details

    All discarded values below `left` are feasible; all discarded values above `right` are infeasible. At termination, `left = right + 1`, so `right` is the last feasible value. Python integer multiplication avoids fixed-width overflow. The same loop handles `0`, `1`, and perfect squares.

    ## Time and Space Complexity

    Time is $O(\log(x+1))$ and auxiliary space is $O(1)$ under the problem’s bounded-integer model.

    ## Key Takeaways

    For a last-true binary search, the final right boundary is the answer.

## Python solution

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        left, right = 0, x
        while left <= right:
            mid = (left + right) // 2
            if mid * mid <= x:
                left = mid + 1
            else:
                right = mid - 1
        return right
```

---

[Back to Day 59](index.md) · [All Algorithm notes](../index.md)
