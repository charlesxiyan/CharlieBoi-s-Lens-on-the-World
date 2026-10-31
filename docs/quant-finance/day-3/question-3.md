# Day 3 · Multiple of Six

*Edited by Charles*

**Probability · Modular arithmetic · Geometric waiting time**

[Original question on OpenQuant](https://openquant.co/questions/multiple-of-six)

## Problem

Roll a fair six-sided die repeatedly and keep a running sum. Stop the first time,
**after at least one roll**, that the sum is divisible by $6$.
What is the expected number of rolls?

!!! tip "Core idea"
    Whatever the current remainder modulo $6$, exactly one of the six faces
    makes the next sum divisible by $6$.

## Short solution

**1. Track the remainder, not the entire sum.** If the current sum has remainder
$r$, the next roll must satisfy $d\equiv-r\pmod6$. The faces $1,\ldots,6$
cover all six residue classes exactly once.

| Current remainder | Next face that ends the game |
| --- | --- |
| $0$ | $6$ |
| $1$ | $5$ |
| $2$ | $4$ |
| $3$ | $3$ |
| $4$ | $2$ |
| $5$ | $1$ |

**2. Find the conditional stopping probability.** Given any history that has
not ended the game, the next roll stops it with probability $1/6$.
Consequently, for the stopping time $T$,

$$
P(T>k)=\left(\frac56\right)^k,\qquad k=0,1,2,\ldots
$$

and

$$
P(T=k)=\left(\frac56\right)^{k-1}\frac16,\qquad k\ge1.
$$

**3. Sum the survival probabilities.**

$$
E[T]=\sum_{k=0}^{\infty}P(T>k)
=\sum_{k=0}^{\infty}\left(\frac56\right)^k
=\frac1{1-5/6}.
$$

!!! success "Answer"
    $$\boxed{E[T]=6\text{ rolls}}$$

!!! warning "Watch out"
    The initial sum is zero, but stopping is checked only after rolling.
    Also, the target is any multiple of six, not just the sum $6$.

??? note "A short example and a recursion check"
    Rolls $2,3,1$ produce sums $2,5,6$ and stop on roll three.
    After the first roll, the winning face is $4$; after the second, it is $1$.
    The winning face changes, but its probability remains $1/6$.

    The remaining waiting time has the same distribution at every surviving
    state. If its expectation is $E$, one roll is always taken and another
    expected $E$ rolls are needed with probability $5/6$:

    $$E=1+\frac56E\quad\Longrightarrow\quad E=6.$$

---

[Previous question](question-2.md) · [Day 3 overview](index.md) · [Next question](question-4.md)
