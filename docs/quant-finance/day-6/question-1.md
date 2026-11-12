# Day 6 · 3 Die Minimum

*Edited by Charles and Sophia*

**Statistics · Order Statistics · Tail-sum formula**

[Original question on OpenQuant](https://openquant.co/questions/3-die-minimum)

## Problem

Roll three independent fair 100-sided dice, each numbered from $1$ to $100$.
Let $X$ be the smallest of the three results. Find $E[X]$.

!!! tip "Core idea"
    Count how many integer thresholds the minimum reaches. The minimum reaches
    a threshold $k$ exactly when all three dice reach it.

## Short solution

### 1. Explain why expectation becomes a sum of tail probabilities

If an outcome is $X=3$, it reaches exactly three positive integer thresholds:
$1$, $2$, and $3$. More generally, for any outcome $X=x$, exactly $x$ of the
statements $X\ge1, X\ge2,\ldots,X\ge100$ are true.

Define an indicator $\mathbf{1}_{\{X\ge k\}}$ to be $1$ when $X\ge k$ and $0$
otherwise. Then, for every possible outcome,

$$X=\sum_{k=1}^{100}\mathbf{1}_{\{X\ge k\}}.$$

For example, $X=3$ makes the right side $1+1+1+0+\cdots+0=3$.
Taking expectations and using linearity gives the missing intermediate step:

$$
\begin{aligned}
E[X]
&=E\left[\sum_{k=1}^{100}\mathbf{1}_{\{X\ge k\}}\right]\\
&=\sum_{k=1}^{100}E\left[\mathbf{1}_{\{X\ge k\}}\right]\\
&=\sum_{k=1}^{100}\left(1\cdot P(X\ge k)+0\cdot P(X<k)\right)\\
&=\sum_{k=1}^{100}P(X\ge k).
\end{aligned}
$$

The indicators need not be independent. Linearity of expectation is enough.
This tail-sum identity applies to nonnegative integer-valued variables; here
the sum stops at 100 because larger thresholds cannot be reached.

### 2. Calculate the probability of reaching each threshold

Write $X=\min(D_1,D_2,D_3)$. The event $X\ge k$ means **all three dice** show
at least $k$. A single die has $101-k$ qualifying faces, namely $k,\ldots,100$:

$$P(D_j\ge k)=\frac{101-k}{100}.$$

By independence of the three rolls,

$$P(X\ge k)=\left(\frac{101-k}{100}\right)^3.$$

As a check, $P(X\ge1)=1$, while $P(X\ge100)=1/100^3$ because all three dice
must show 100.

### 3. Sum the probabilities

Substitute and reverse the summation order with $j=101-k$:

$$
E[X]=\frac1{100^3}\sum_{j=1}^{100}j^3
=\frac1{100^3}\left(\frac{100\cdot101}{2}\right)^2.
$$

Here we used $\sum_{j=1}^{n}j^3=[n(n+1)/2]^2$.

!!! success "Answer"
    $$\boxed{E[X]=\frac{25{,}502{,}500}{1{,}000{,}000}=25.5025}$$

!!! warning "Watch out"
    $P(X\ge k)$ is not $P(X=k)$. An outcome $X=3$ contributes to three tail
    events, and that is precisely how its value of three is counted. Also,
    counting faces from $k$ through 100 requires the inclusive count $101-k$.

??? note "The same identity starting from the usual definition of expectation"
    Expand each weight $x$ as a sum of $x$ ones and exchange the finite sums:

    $$
    \begin{aligned}
    E[X]&=\sum_{x=1}^{100}xP(X=x)\\
    &=\sum_{x=1}^{100}\sum_{k=1}^{x}P(X=x)\\
    &=\sum_{k=1}^{100}\sum_{x=k}^{100}P(X=x)\\
    &=\sum_{k=1}^{100}P(X\ge k).
    \end{aligned}
    $$

    For a fixed $k$, the inner sum adds the probabilities of all values
    $k,k+1,\ldots,100$, which is exactly $P(X\ge k)$.

---

[Day 6 overview](index.md) · [Next question](question-2.md)
