---
hide:
  - toc
---

# 190. Reverse Bits

*Edited by Charles*

**Easy · Python · Binary Manipulation · Bit positions**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/reverse-bits/description/).

    Reverse the order of all 32 bits in the input integer’s 32-bit representation, including leading zero bits.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    The bit at index `i` from the right moves to index `31 - i`. Extract it using a right shift and mask, then place it with a left shift and OR.

    ## Algorithm Steps

    1. Start with a zero result.
    2. For each `i` from `0` through `31`, compute `(n >> i) & 1`.
    3. Move that bit to `31 - i` and OR it into the result.
    4. Return the completed 32-bit reversal.

    ## Hand Simulation

    For `n = 5`, the last three bits are `101`. Bit `0` moves to bit `31`, and bit `2` moves to bit `29`; all other bits are zero. The result is $2^{31}+2^{29}=2684354560$.

    ## Important Implementation Details

    Always process 32 positions. Stopping at the highest set bit would lose the effect of leading zeros on the reversed value. `& 1` isolates exactly one bit; `|=` sets its destination without clearing earlier bits. Python integers can hold the resulting value without signed 32-bit overflow.

    ## Time and Space Complexity

    Time is $O(32)=O(1)$ and auxiliary space is $O(1)$ under the fixed 32-bit model.

    ## Key Takeaways

    Bit reversal is a fixed-position permutation, not reversal of the shortest binary string.

## Python solution

```python
class Solution:
    def reverseBits(self, n: int) -> int:
        res = 0
        for i in range(32):
            bit = (n >> i) & 1
            res |= bit << (31 - i)
        return res
```

---

[Back to Day 57](index.md) · [All Algorithm notes](../index.md)
