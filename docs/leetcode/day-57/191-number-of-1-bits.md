---
hide:
  - toc
---

# 191. Number of 1 Bits

*Edited by Charles*

**Easy · Python · Binary Manipulation · Binary digits**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/number-of-1-bits/description/).

    Count the set bits (`1` digits) in the binary representation of the given nonnegative integer.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Division by two removes the lowest binary digit. The remainder tells whether that removed digit was `1`.

    ## Algorithm Steps

    1. Initialize a zero count.
    2. While `n > 0`, increase the count if `n % 2` is nonzero.
    3. Remove that digit using `n //= 2`.
    4. Return the count.

    ## Hand Simulation

    For `n = 13` (`1101`): `13` is odd → count `1`, next `6`; `6` is even → count `1`, next `3`; `3` is odd → count `2`, next `1`; `1` is odd → count `3`, next `0`. Return `3`.

    ## Important Implementation Details

    This supplied implementation uses arithmetic rather than bitwise operators. For nonnegative integers, `% 2` corresponds to `& 1`, and `// 2` corresponds to `>> 1`. Zero naturally returns zero. Negative integers are outside the intended input model.

    ## Time and Space Complexity

    For $B$ binary digits, time is $O(B)$ and auxiliary space is $O(1)$ in the usual fixed-width arithmetic model. For 32-bit inputs there are at most 32 iterations.

    ## Key Takeaways

    Parity reveals the lowest bit; division by two advances to the next one.

## Python solution

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n > 0:
            if n % 2:
                count += 1
            n //= 2
        return count
```

---

[Back to Day 57](index.md) · [All Algorithm notes](../index.md)
