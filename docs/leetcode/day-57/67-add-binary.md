---
hide:
  - toc
---

# 67. Add Binary

*Edited by Charles*

**Easy · Python · Binary Manipulation · Carry**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/add-binary/description/).

    Add two strings representing nonnegative binary integers and return their sum as a binary string.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Perform column addition from right to left. A column total contributes `total % 2` to the answer and passes `total // 2` to the next column.

    ## Algorithm Steps

    1. Put one pointer at the end of each string and initialize `carry = 0`.
    2. Add available digits and the previous carry.
    3. Append the result bit and compute the new carry.
    4. Continue until both strings and the carry are exhausted, then reverse the result.

    ## Hand Simulation

    For `a = "11"`, `b = "1"`:

    | Column | Total | Appended bit | New carry |
    | --- | --- | --- | --- |
    | Rightmost | `1 + 1 = 2` | `0` | `1` |
    | Next | `1 + 1 = 2` | `0` | `1` |
    | Final carry | `1` | `1` | `0` |

    Reverse `["0","0","1"]` to get `"100"`.

    ## Important Implementation Details

    The `or carry` condition preserves an overflow bit. Independent pointer checks handle unequal string lengths. Appending to a list avoids repeatedly prepending to an immutable string.

    ## Time and Space Complexity

    For maximum input length $L$, time and result storage are $O(L)$. The list and reversed copy also use $O(L)$ auxiliary space.

    ## Key Takeaways

    The same carry rule works in any base: remainder gives the current digit, integer division gives the carry.

## Python solution

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        res = []
        curA, curB = len(a) - 1, len(b) - 1
        carry = 0
        while curA >= 0 or curB >= 0 or carry:
            total = carry
            if curA >= 0:
                total += int(a[curA])
                curA -= 1
            if curB >= 0:
                total += int(b[curB])
                curB -= 1
            res.append(str(total % 2))
            carry = total // 2
        return ''.join(res[::-1])
```

---

[Back to Day 57](index.md) · [All Algorithm notes](../index.md)
