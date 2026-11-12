# Day 2 · Ten Floor Building

*Edited by Charles and Sophia*

**Probability · Indicators & linearity of expectation**

[Original question on OpenQuant](https://openquant.co/questions/ten-floor-building)

## Problem

A building has 10 floors above the basement. Twelve people get into an elevator at the basement,
and each independently picks a floor uniformly at random to get out.
How many floors do you expect the elevator to **stop** at?

!!! tip "Core idea"
    Don't count stops directly — split the total into one indicator per floor.
    Linearity of expectation then reduces the problem to a single floor's probability.

## Short solution

**1. Split into indicators.** Let $I_j=1$ if the elevator stops at floor $j$, else $0$:

$$
S=\sum_{j=1}^{10} I_j,
\qquad
\mathbb{E}[S]=\sum_{j=1}^{10}\mathbb{E}[I_j].
$$

**2. Look at one floor.** The elevator skips floor $j$ only if all 12 people choose another floor:

$$
P(I_j=0)=\left(\frac{9}{10}\right)^{12}
\quad\Longrightarrow\quad
\mathbb{E}[I_j]=1-\left(\frac{9}{10}\right)^{12}.
$$

**3. Use symmetry.** All 10 floors are alike, so

$$
\mathbb{E}[S]=10\left[1-\left(\frac{9}{10}\right)^{12}\right].
$$

!!! success "Answer"
    $$\boxed{\mathbb{E}[S]=10\left[1-\left(\tfrac{9}{10}\right)^{12}\right]\approx 7.18\text{ stops}}$$

!!! warning "Watch out"
    $S$ is **not** binomial: the $I_j$ are dependent (if one floor is skipped, the
    others become more likely to be used). Linearity of expectation doesn't need
    independence, so the mean is still $10\,p$ — but the variance is not $10\,p(1-p)$.

??? note "Generalization: k people, n floors"
    With $k$ people choosing uniformly among $n$ floors,

    $$
    \mathbb{E}[S]=n\left[1-\left(1-\frac{1}{n}\right)^{k}\right].
    $$

    Sanity checks: $k=1$ gives $1$; $k\to\infty$ gives $n$;
    and $k=n$ large gives $\approx n\,(1-1/e)\approx 0.632\,n$.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 1 of 4 |
    | Topic | Probability |
    | Difficulty | Medium |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | November 5, 2026 |
    | Source | [OpenQuant: Ten Floor Building](https://openquant.co/questions/ten-floor-building) |

---

[Day 2 overview](index.md) · [Next question](question-2.md)
