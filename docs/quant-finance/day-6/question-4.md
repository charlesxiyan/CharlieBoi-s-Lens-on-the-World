# Day 6 · More Heads

*Edited by Charles and Sophia*

**Probability · Conditional Expectation · Geometric Distribution**

[Original question on OpenQuant](https://openquant.co/questions/more-heads)

## Problem

Alice and Bob each flip a coin until their first head. Given that Bob needed
strictly more flips than Alice, what is the expected number of Alice's flips?

Assume both coins are fair, all flips are independent, and the count includes
the final head. These assumptions make the source's coin experiment precise.

!!! tip "Core idea"
    First fix Alice's count at $k$. For Bob to need more than $k$ flips, his
    first $k$ flips must all be tails. Calculate that joint event, then
    normalize over all possible values of $k$.

## Short solution

### 1. Name the counts and the information we are given

Let $A$ be Alice's flip count and $B$ Bob's. We want

$$E[A\mid B>A].$$

Without the condition, $E[A]=2$. However, knowing Alice finished sooner changes
which values of $A$ are likely. A tie $B=A$ does not satisfy the condition.

### 2. Calculate the chance that Alice takes k flips and Bob takes more

For Alice to finish on flip $k$, she must get $k-1$ tails followed by a head:

$$P(A=k)=\left(\frac12\right)^{k-1}\frac12=\frac1{2^k}.$$

For Bob to take **more than** $k$ flips, his first $k$ flips must all be tails:

$$P(B>k)=\left(\frac12\right)^k=\frac1{2^k}.$$

Bob need not get a head on flip $k+1$; any later first head qualifies.
The two people's flips are independent, so

$$P(A=k,\ B>A)=P(A=k)P(B>k)=\frac1{4^k}.$$

### 3. Divide by the probability of the condition

The events above are disjoint across Alice's possible counts $k=1,2,\ldots$.
Their sum is the probability that Bob finishes later:

$$P(B>A)=\sum_{k=1}^{\infty}\frac1{4^k}
=\frac{1/4}{1-1/4}=\frac13.$$

Therefore,

$$
P(A=k\mid B>A)
=\frac{P(A=k,\ B>A)}{P(B>A)}
=\frac{3}{4^k}
=\frac34\left(\frac14\right)^{k-1}.
$$

| Alice's count | Joint probability $P(A=k, B>A)$ | Conditional probability $P(A=k\mid B>A)$ |
| --- | --- | --- |
| $1$ | $1/4$ | $3/4$ |
| $2$ | $1/16$ | $3/16$ |
| $3$ | $1/64$ | $3/64$ |

Given that Alice finished sooner, she used just one flip with probability $3/4$.
Longer waits are progressively less likely.

### 4. Take the mean of this conditional distribution

The conditional probabilities have the geometric form $p(1-p)^{k-1}$ with
$p=3/4$, counting trials starting at one. Its mean is $1/p$.

!!! success "Answer"
    $$\boxed{E[A\mid B>A]=\frac1{3/4}=\frac43\text{ flips}}$$

!!! warning "Watch out"
    $B>k$ means $k$ initial tails, whereas $B\ge k$ means only $k-1$ initial
    tails. Also, symmetry does not give $P(B>A)=1/2$: ties have positive
    probability. The normalization above accounts for them.

??? note "Direct expectation calculation without quoting the geometric mean"
    Let $S=r+2r^2+3r^3+\cdots$ for $0<r<1$. Multiplying by $r$ and subtracting
    yields

    $$S-rS=r+r^2+r^3+\cdots=\frac{r}{1-r},$$

    so $S=r/(1-r)^2$. Taking $r=1/4$ gives

    $$E[A\mid B>A]=\sum_{k=1}^{\infty}k\frac3{4^k}
    =3\frac{1/4}{(1-1/4)^2}=\frac43.$$

??? note "Why the first-head race gives the same answer"
    Imagine pairing Alice's and Bob's flips into rounds until at least one
    gets a head. In each round:

    | Pair | Probability | Interpretation |
    | --- | --- | --- |
    | TT | $1/4$ | Both continue. |
    | HT | $1/4$ | Alice finishes first: $B>A$. |
    | TH | $1/4$ | Bob finishes first. |
    | HH | $1/4$ | They tie. |

    The first non-TT round has index $M=\min(A,B)$ and occurs with per-round
    probability $3/4$, so $E[M]=4/3$. To justify using this after conditioning,
    note that the terminating type (HT, TH, or HH) is equally likely regardless
    of how many TT rounds preceded it. Thus the stopping round is independent
    of which terminating type occurs. Conditional on HT, $A=M$ and still has
    mean $4/3$.

    Merely observing that $A=\min(A,B)$ when $B>A$ would not, by itself,
    justify replacing a conditional expectation with the unconditional one.

---

[Previous question](question-3.md) · [Day 6 overview](index.md)
