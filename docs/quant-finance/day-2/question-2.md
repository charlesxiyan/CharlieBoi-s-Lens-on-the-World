# Day 2 · Conditional Die Rolls

*Edited by Charles and Sophia*

**Probability · Conditioning & geometric distributions**

[Original question on OpenQuant](https://openquant.co/questions/conditional-die-rolls)

## Problem

Dan rolls a fair die until he gets a 6. Let $N$ be the number of rolls.

1. Given that he did **not** see a 5, what is $\mathbb{E}[N]$?
2. Given that he **did** see a 5, what is $\mathbb{E}[N]$?
3. Given that he saw **only even numbers**, what is $\mathbb{E}[N]$?

!!! tip "Core idea"
    Conditioning on "no 5 before the 6" doesn't make the die a 5-sided die.
    It reweights every path by the same constant $1/P(\text{condition})$,
    and the result is still geometric — just with a **larger** success probability.

## Short solution

### Part 1 · No 5

**1. Probability of the condition.** Among $\{5,6\}$, each is equally likely to appear first:

$$
P(A)=P(\text{no 5 before the 6})=\frac{1}{2}.
$$

**2. Condition on the whole path.** A path of length $n$ with no 5 is
$n-1$ rolls from $\{1,2,3,4\}$, then a 6:

$$
P(N=n\mid A)
=\frac{P(N=n,\,A)}{P(A)}
=\frac{\left(\frac{4}{6}\right)^{n-1}\cdot\frac{1}{6}}{1/2}
=\left(\frac{2}{3}\right)^{n-1}\cdot\frac{1}{3}.
$$

**3. Read off the geometric.** This is $\text{Geometric}(1/3)$, so $\mathbb{E}[N\mid A]=3$.

??? question "Why can we just divide the 1/6 by 1/2?"
    The factor $1/P(A)=2$ is applied **once per path**, not once per roll.
    Every path in $A$ ends in exactly one 6, so we may attach that factor to the last roll:
    $\tfrac16\cdot 2=\tfrac13$. The non-6 rolls keep their weight $\tfrac46=\tfrac23$.
    Since $\tfrac23+\tfrac13=1$, the conditional law is again a proper geometric distribution.

### Part 2 · Saw a 5

**1. Use total expectation.** Unconditionally $\mathbb{E}[N]=6$, and $A$, $A^c$ each have probability $\tfrac12$:

$$
6=\tfrac12\cdot 3+\tfrac12\cdot\mathbb{E}[N\mid A^c]
\quad\Longrightarrow\quad
\mathbb{E}[N\mid A^c]=9.
$$

**2. Same answer, step by step.** Let $T$ be the first roll showing a 5 or a 6.
Given a 5 comes first, $\mathbb{E}[T]=3$; after that, a fresh wait for a 6 adds $6$:

$$
\mathbb{E}[N\mid A^c]=3+6=9.
$$

??? question "Why is it still 3 rolls to reach the 5, given that we saw a 5?"
    $T\sim\text{Geometric}(1/3)$, and **which** face it lands on (5 or 6) is independent
    of **when** it happens: at every roll, a hit is a 5 or a 6 with probability $\tfrac12$ each.
    So conditioning on "the hit was a 5" does not change the law of $T$, and $\mathbb{E}[T\mid A^c]=3$.
    After the 5, the condition is already satisfied and future rolls are fresh, so the remaining wait is $6$.

### Part 3 · Only even numbers

**1. Probability of the condition.** The 6 must come first among $\{1,3,5,6\}$:

$$
P(C)=\frac{1}{4}.
$$

**2. Same reweighting as Part 1.** Paths in $C$ are $n-1$ rolls from $\{2,4\}$, then a 6:

$$
P(N=n\mid C)
=\frac{\left(\frac{2}{6}\right)^{n-1}\cdot\frac{1}{6}}{1/4}
=\left(\frac{1}{3}\right)^{n-1}\cdot\frac{2}{3}.
$$

**3. Read off the geometric.** This is $\text{Geometric}(2/3)$, so $\mathbb{E}[N\mid C]=\tfrac32$.

!!! success "Answer"
    $$\boxed{\mathbb{E}[N\mid\text{no }5]=3,\qquad \mathbb{E}[N\mid\text{saw }5]=9,\qquad \mathbb{E}[N\mid\text{only evens}]=\frac{3}{2}}$$

!!! warning "Watch out"
    The tempting answer for Part 3 is $3$, treating the die as having faces $\{2,4,6\}$.
    That is wrong: the condition favors **short** runs, because every extra roll is
    another chance to see an odd number. The same trap gives $5$ instead of $3$ in Part 1.

??? note "Generalization: avoid b forbidden faces"
    If the condition is "none of $b$ specific faces appears before the 6", then $N$ equals the
    first time any of those $b+1$ faces appears, and that time is independent of which face it is:

    $$
    \mathbb{E}[N\mid\text{no forbidden face}]=\frac{6}{b+1}.
    $$

    Part 1: $b=1\Rightarrow 3$. Part 3: $b=3\Rightarrow \tfrac32$.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 2 of 4 |
    | Topic | Probability |
    | Difficulty | Medium |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | November 5, 2026 |
    | Source | [OpenQuant: Conditional Die Rolls](https://openquant.co/questions/conditional-die-rolls) |

---

[Previous question](question-1.md) · [Day 2 overview](index.md) · [Next question](question-3.md)
