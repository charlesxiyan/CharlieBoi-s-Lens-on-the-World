# Day 3 · Digit Sum of 19

*Edited by Charles*

**Discrete mathematics · Stars and bars · Inclusion–exclusion**

[Original question on OpenQuant](https://openquant.co/questions/digit-sum-of-19)

## Problem

Count the positive integers smaller than $1{,}000{,}000$ whose decimal digits
sum to $19$.

!!! tip "Core idea"
    Pad every number to six digits. Count nonnegative solutions with sum $19$,
    then subtract those with a digit greater than $9$.

## Short solution

**1. Use six positions, allowing leading zeros.** For example, $199$ becomes
$000199$. Every integer from $0$ through $999999$ has exactly one such
representation. Zero has digit sum zero, so it is excluded automatically.
We need

$$d_1+\cdots+d_6=19,\qquad 0\le d_i\le9.$$

**2. Temporarily remove the upper bounds.** Stars and bars gives

$$\binom{19+6-1}{6-1}=\binom{24}{5}=42{,}504.$$

**3. Subtract invalid digits.** If one specified digit is at least $10$,
subtract $10$ from it. The six nonnegative variables now sum to $9$, giving

$$\binom{9+6-1}{6-1}=\binom{14}{5}=2{,}002.$$

There are six choices of that digit. Two digits cannot both be at least $10$
because their sum would already exceed $19$, so these invalid sets do not overlap.

**4. Finish the count.**

$$N=\binom{24}{5}-6\binom{14}{5}=42{,}504-12{,}012.$$

!!! success "Answer"
    $$\boxed{N=30{,}492}$$

!!! warning "Watch out"
    Counting only six-digit numbers misses shorter valid integers.
    Ordinary stars and bars also counts impossible digits such as $10$ or $19$;
    the upper-bound correction is essential.

??? note "Independent check: count digit sums with dynamic programming"
    Start with one empty sequence of sum zero. Append each possible digit to
    every sequence counted so far. After six positions, read the count at $19$.

    ```python
    counts = [1] + [0] * 19
    for _ in range(6):
        nxt = [0] * 20
        for total, ways in enumerate(counts):
            for digit in range(10):
                if total + digit <= 19:
                    nxt[total + digit] += ways
        counts = nxt
    print(counts[19])  # 30492
    ```

---

[Previous question](question-4.md) · [Day 3 overview](index.md) · [Next day](../day-4/index.md)
