---
hide:
  - toc
---

# 201. Bitwise AND of Numbers Range

*Edited by Charles*

**Medium · Python · Binary Manipulation · Common prefix**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/bitwise-and-of-numbers-range/description/).

    Return the bitwise AND of every integer in the inclusive nonnegative range `[left, right]`.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Only the common binary prefix of the endpoints survives. At the highest position where they differ, the range crosses a binary boundary: some values have zero there, and the boundary value also has zeros in every lower position. AND clears that entire suffix.

    ## Algorithm Steps

    1. Right-shift both endpoints until they become equal.
    2. Count how many suffix positions were removed.
    3. Left-shift the common prefix by that count, restoring the cleared positions as zeros.

    ## Hand Simulation

    For `[5,7]`: `101` and `111` differ; shift to `10` and `11`, then to `1` and `1`. Two shifts were needed. Restore two zeros: `1 << 2 = 100`, or `4`. Indeed `5 & 6 & 7 = 4`.

    ## Important Implementation Details

    AND-ing only the endpoints is insufficient: for `[5,7]`, `5 & 7 = 5`, but the intervening `6` clears the lowest bit. Equal endpoints need no shifts. If the common prefix becomes zero, the whole result is zero.

    ## Time and Space Complexity

    For $B$-bit endpoints, time is $O(B)$ and auxiliary space is $O(1)$ under fixed-width arithmetic.

    ## Key Takeaways

    For a complete integer interval, the changing suffix vanishes under AND; preserve only the shared prefix.

## Python solution

```python
class Solution:
    def rangeBitwiseAnd(self, left: int, right: int) -> int:
        shift = 0
        while left < right:
            left >>= 1
            right >>= 1
            shift += 1
        return left << shift
```

---

[Back to Day 58](index.md) · [All Algorithm notes](../index.md)
