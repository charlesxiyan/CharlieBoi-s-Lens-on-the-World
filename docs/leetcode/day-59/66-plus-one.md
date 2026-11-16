---
hide:
  - toc
---

# 66. Plus One

*Edited by Charles*

**Easy · Python · Math · Carry · Arrays**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/plus-one/description/).

    An array stores the decimal digits of a nonnegative integer, most significant first. Return the digits after adding one.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Only a trailing run of nines propagates a carry. Replace each such nine with zero; the first smaller digit absorbs the carry by increasing by one.

    ## Algorithm Steps

    1. Scan digits from right to left.
    2. If the current digit is below nine, increment it and return.
    3. Otherwise set it to zero and continue carrying.
    4. If all digits were nines, prepend a new leading one.

    ## Hand Simulation

    For `[1,2,9]`, change the last `9` to `0`, then increment `2` to `3`: `[1,3,0]`. For `[9,9]`, both digits become zero and a leading one is added: `[1,0,0]`.

    ## Important Implementation Details

    The code mutates `digits`. The ordinary case returns that same list; the all-nines case returns a newly allocated `[1] + digits`. No conversion of the whole input to an integer is needed.

    ## Time and Space Complexity

    Worst-case time is $O(N)$. Working space is $O(1)$ until overflow; the all-nines case allocates an $O(N)$ result list.

    ## Key Takeaways

    Carry propagation stops as soon as a digit can absorb it.

## Python solution

```python
class Solution:
    def plusOne(self, digits: list[int]) -> list[int]:
        for i in range(len(digits) - 1, -1, -1):
            if digits[i] < 9:
                digits[i] += 1
                return digits
            digits[i] = 0
        return [1] + digits
```

---

[Back to Day 59](index.md) · [All Algorithm notes](../index.md)
