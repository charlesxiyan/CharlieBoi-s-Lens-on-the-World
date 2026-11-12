# Day 3 · Multiple of Six

*Edited by Charles and Sophia*

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

??? note "State reduction: from six states to two"
    **Original states.** The natural state is the running sum modulo $6$, giving six
    residues $0,1,2,3,4,5$. From any residue $r$, the next roll $d\in\{1,\ldots,6\}$
    sends the sum to $r+d \pmod 6$, which hits each residue exactly once.
    So **every row of the transition matrix is the same**:

    $$
    P(r\to s)=\frac16\qquad\text{for all } r,s\in\{0,1,\ldots,5\}.
    $$

    **Merge the non-stopping states.** Only one distinction matters for the game:
    residue $0$ means **stop**, and residues $1,\ldots,5$ mean **continue**.
    Every continuing state moves in exactly the same way:

    $$
    P(\text{continue}\to 0)=\frac16,
    \qquad
    P(\text{continue}\to\{1,\ldots,5\})=\frac56.
    $$

    The starting state (sum $0$, no roll yet) behaves the same way, since we only
    check for stopping after a roll. So it also belongs to "continue".

    **The reduced chain.** Two states are enough:

    | From \ To | Continue | Stop |
    | --- | --- | --- |
    | Continue | $5/6$ | $1/6$ |
    | Stop | — | absorbing |

    $T$ is the first time the chain enters **Stop**, so
    $T\sim\text{Geometric}(1/6)$ and $E[T]=6$.

    **Why merging is allowed.** States $1,\ldots,5$ can be lumped into one state
    because they all have the **same probability of moving into each group**:
    $1/6$ into Stop and $5/6$ back into Continue. Once in Continue, the exact residue
    never changes the future of the game. If, for example, residue $3$ had a higher
    chance of stopping than residue $1$, this merge would fail and we would have to
    keep all six states.

---

[Previous question](question-2.md) · [Day 3 overview](index.md) · [Next question](question-4.md)
