---
hide:
  - toc
---

# 136. Single Number

*Edited by Charles*

**Easy · Python · Binary Manipulation · XOR**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/single-number/description/).

    Every integer appears twice except one integer that appears once. Find the single integer in linear time with constant auxiliary space.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    XOR cancels equal pairs: $a\oplus a=0$, while $a\oplus0=a$. XOR is associative and commutative, so paired values cancel regardless of their positions.

    ## Algorithm Steps

    1. Initialize `res = 0`.
    2. XOR every value into `res`.
    3. Return the uncancelled value.

    ## Hand Simulation

    For `[4,1,2,1,2]`, the running results are `4,5,7,6,4`. Equivalently, regroup as `4 ^ (1 ^ 1) ^ (2 ^ 2) = 4`.

    ## Important Implementation Details

    The exact multiplicity condition matters: XOR tracks parity, so this method does not solve the version where other values occur three times. Zero and negative values also cancel correctly in Python.

    ## Time and Space Complexity

    For $N$ fixed-width integers, time is $O(N)$ and auxiliary space is $O(1)$.

    ## Key Takeaways

    When every unwanted value appears an even number of times, XOR can remove those values without a frequency table.

## Python solution

```python
class Solution:
    def singleNumber(self, nums: list[int]) -> int:
        res = 0
        for num in nums:
            res ^= num
        return res
```

---

[Back to Day 58](index.md) · [All Algorithm notes](../index.md)
