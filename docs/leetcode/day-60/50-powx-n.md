---
hide:
  - toc
---

# 50. Pow(x, n)

*Edited by Charles*

**Medium · Python · Math · Fast exponentiation · Recursion**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/powx-n/description/).

    Compute `x` raised to integer exponent `n`, including negative exponents for nonzero `x`.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Compute one half-power and reuse it. For $n=2m$, $x^n=(x^m)^2$; for $n=2m+1$, $x^n=x(x^m)^2$. Each recursive call halves the exponent.

    ## Algorithm Steps

    1. Return `1.0` for exponent zero.
    2. Recursively compute `half = quickPow(x, n // 2)` once.
    3. Square `half`, multiplying by `x` once more if the exponent is odd.
    4. For a negative original exponent, compute the positive power and take its reciprocal.

    ## Hand Simulation

    For `x = 2`, `n = 5`, recursive exponents are `5 → 2 → 1 → 0`. Returning upward gives `1`, `2`, `4`, then `2 × 4 × 4 = 32`. For `n = -3`, compute `2^3 = 8` and return `1/8`.

    ## Important Implementation Details

    Saving `half` avoids making two identical recursive calls. Convert a negative exponent before recursion: repeatedly floor-dividing a negative exponent would not reach zero correctly. The input excludes zero raised to a negative exponent. Floating-point rounding remains possible; this algorithm is not exact rational arithmetic.

    ## Time and Space Complexity

    Time and recursion-stack space are $O(\log(|n|+1))$, with constant work for exponent zero.

    ## Key Takeaways

    Reuse a repeated subproblem and reduce the exponent geometrically.

## Python solution

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        def quickPow(x, n):
            if n == 0:
                return 1.0
            half = quickPow(x, n // 2)
            if n % 2 == 0:
                return half * half
            else:
                return x * half * half
        if n < 0:
            return 1 / quickPow(x, -n)
        return quickPow(x, n)
```

---

[Back to Day 60](index.md) · [All Algorithm notes](../index.md)
