---
hide:
  - toc
---

# 172. Factorial Trailing Zeroes

*Edited by Charles*

**Medium · Python · Math · Prime factors**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/factorial-trailing-zeroes/description/).

    Count the trailing decimal zeroes of `n!`, without computing the factorial.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Each trailing zero needs a factor of ten, or one factor of two paired with one factor of five. A factorial contains at least as many twos as fives, so count the fives. Multiples of 25 contribute an additional five; multiples of 125 contribute another, and so on.

    ## Algorithm Steps

    1. Divide `n` by five using integer division and add the quotient.
    2. Repeat with that quotient to count additional factors from higher powers of five.
    3. Stop when `n` becomes zero.

    ## Hand Simulation

    For `n = 30`, the first quotient is `6`, counting `5,10,15,20,25,30`. The next quotient is `1`, counting the extra factor in `25`. The next is `0`. The answer is `6 + 1 = 7`.

    ## Important Implementation Details

    The formula is $\lfloor n/5\rfloor+\lfloor n/25\rfloor+\lfloor n/125\rfloor+\cdots$. Repeated division computes these terms without explicitly constructing powers. Counting only multiples of five misses extra factors. For `n = 0`, the answer is zero because $0!=1$.

    ## Time and Space Complexity

    Time is $O(\log_5(n+1))$ and auxiliary space is $O(1)$ in the usual arithmetic model.

    ## Key Takeaways

    Count prime factors rather than constructing a huge product.

## Python solution

```python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        count = 0
        while n:
            n //= 5
            count += n
        return count
```

---

[Back to Day 59](index.md) · [All Algorithm notes](../index.md)
