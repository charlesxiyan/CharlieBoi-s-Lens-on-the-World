# Day 1 · Even Before Odd

*Edited by Charles and Sophia*

**Probability · Symmetry & counting**

[Original question on OpenQuant](https://openquant.co/questions/even-before-odd)

## Problem

Roll a fair 6-sided die until all 6 faces have appeared.
What is the probability that **no odd face** appears until **all three even faces** have been seen?

!!! tip "Core idea"
    Repeated rolls carry no information. Only the order in which the six faces
    make their **first appearance** matters, and by symmetry every order is equally likely.

## Short solution

**1. Reduce to first appearances.** Record each face only the first time it shows up.
This turns the roll sequence into a permutation of $\{1,2,3,4,5,6\}$:

$$
\text{rolls: } 4,\,4,\,2,\,6,\,2,\,3,\,\dots
\quad\longrightarrow\quad
4\;|\;2\;|\;6\;|\;3\;|\;\cdots
$$

**2. Use symmetry.** No face is favored, so all $6!$ first-appearance orders are equally likely.

**3. Count the good orders.** The first three new faces must be $\{2,4,6\}$ in some order,
followed by $\{1,3,5\}$ in some order:

$$
P=\frac{3!\cdot 3!}{6!}=\frac{36}{720}.
$$

!!! success "Answer"
    $$\boxed{P=\frac{1}{20}=0.05}$$

!!! warning "Watch out"
    Rolling an even face twice is fine — the condition only restricts **new** faces.
    Equivalently, each new face must be even:
    $\frac{3}{6}\cdot\frac{2}{5}\cdot\frac{1}{4}=\frac{1}{20}$.
    Don't use $\left(\tfrac{1}{2}\right)^3$; that ignores the shrinking pool of unseen faces.

??? note "Generalization: one group before another"
    For $a$ faces in group A and $b$ faces in group B, the probability that all of
    A first-appear before any of B is

    $$
    P=\frac{a!\,b!}{(a+b)!}=\frac{1}{\binom{a+b}{a}}.
    $$

    Here $a=b=3$, so $P=1/\binom{6}{3}=1/20$.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 1 of 4 |
    | Topic | Probability |
    | Difficulty | Easy |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | October 22, 2026 |
    | Source | [OpenQuant: Even Before Odd](https://openquant.co/questions/even-before-odd) |

---

[Day 1 overview](index.md) · [Next question](question-2.md)
