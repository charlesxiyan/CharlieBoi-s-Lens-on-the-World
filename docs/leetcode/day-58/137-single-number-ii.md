---
hide:
  - toc
---

# 137. Single Number II

*Edited by Charles*

**Medium · Python · Binary Manipulation · Bit counting · Two’s complement**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/single-number-ii/description/).

    Every integer appears three times except one that appears once. Recover that single signed 32-bit integer using constant auxiliary space.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Count the contribution to each bit independently. A triplicated value contributes either zero or three set bits at a position. Taking the count modulo three leaves exactly the unique value’s bit.

    ## Algorithm Steps

    1. For each of 32 positions, sum `(num >> i) & 1` across the array.
    2. If the count modulo three is nonzero, set that bit in `res`.
    3. Interpret the assembled 32-bit pattern as signed: if bit 31 is set, subtract $2^{32}$.

    ## Hand Simulation

    For `[2,2,3,2]`, write `2 = 10` and `3 = 11`. At bit `0`, the count is `1`, leaving `1`. At bit `1`, the count is `4`, also leaving `1`. Higher counts are zero, so the result is `11`, or `3`.

    For `[-2,-2,-2,-7]`, the surviving low-32-bit pattern is `0xFFFFFFF9`, initially the positive integer `4294967289`. Subtracting `4294967296` recovers `-7`.

    ## Important Implementation Details

    Python right shifts negative integers with sign extension. Masking each of the lowest 32 positions still extracts the required two’s-complement bits. Building `res` with OR produces a nonnegative integer, so the final signed conversion is necessary. The sign threshold is $2^{31}$, not $2^{32}$.

    ## Time and Space Complexity

    Time is $O(32N)=O(N)$ and auxiliary space is $O(1)$ for signed 32-bit inputs.

    ## Key Takeaways

    Modular counting generalizes XOR cancellation, but reconstructing signed values requires handling the sign bit.

## Python solution

```python
class Solution:
    def singleNumber(self, nums: list[int]) -> int:
        res = 0
        for i in range(32):
            count = 0
            for num in nums:
                # Count each number's contribution at this bit position.
                # A triplicated value contributes either three ones or zero.
                # The unique value contributes either one or zero.
                # Modulo three therefore leaves only the unique value's bit.
                count += (num >> i) & 1
            if count % 3:
                # Set this bit in res only when the remaining bit is one.
                res |= (1 << i)
        if res >= (1 << 31):
            res -= (1 << 32)
        return res
```

---

[Back to Day 58](index.md) · [All Algorithm notes](../index.md)
