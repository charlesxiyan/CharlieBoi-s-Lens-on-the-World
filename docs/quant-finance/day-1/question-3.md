# Day 1 · Highest Roll EV

*Edited by Charles and Sophia*

**Probability · Symmetry & expectation**

[Original question on OpenQuant](https://openquant.co/questions/highest-roll-ev)

## Problem

Jim rolls a fair 6-sided die until he gets a 4.
What is the expected value of the **highest number** he rolls during this process?

!!! tip "Core idea"
    The game ends on a 4, so the maximum is at least 4 and rolls of 1, 2, 3 are irrelevant.
    Only the order in which $\{4,5,6\}$ **first appear** matters, and by symmetry every order is equally likely.

## Short solution

**1. Drop the irrelevant faces.** Faces 1, 2, 3 never affect the maximum.
Reduce each game to the first-appearance order of $4,5,6$:

$$
\text{rolls: } 2,\,5,\,1,\,5,\,4
\quad\longrightarrow\quad
5\;|\;4\;|\;(6 \text{ never seen})
$$

**2. Use symmetry.** All $3!=6$ relative orders of $\{4,5,6\}$ are equally likely.
The maximum is the largest face that comes **before** 4:

| First-appearance order | Count | Max |
| --- | --- | --- |
| `4-5-6`, `4-6-5` | 2 | 4 |
| `5-4-6` | 1 | 5 |
| `6-4-5`, `6-5-4`, `5-6-4` | 3 | 6 |

**3. Take the expectation.**

$$
\mathbb{E}[M]=\frac{2\cdot 4+1\cdot 5+3\cdot 6}{6}=\frac{31}{6}.
$$

!!! success "Answer"
    $$\boxed{\mathbb{E}[M]=\frac{31}{6}\approx 5.17}$$

!!! warning "Watch out"
    The game stops **on** the 4, so a 5 or 6 counts only if it shows up before the first 4.
    $P(6 \text{ before } 4)=\tfrac12$, not $\tfrac13$ — compare 6 against 4 alone, ignoring 5.

??? note "Shortcut: tail-sum formula"
    Since $M\ge 4$ always,

    $$
    \mathbb{E}[M]=4+P(M\ge 5)+P(M\ge 6)
    =4+\frac{2}{3}+\frac{1}{2}=\frac{31}{6},
    $$

    where $P(M\ge 5)$ is the chance that 5 or 6 appears before 4 (2 of 3 faces),
    and $P(M\ge 6)$ is the chance that 6 appears before 4.

??? note "Generalization: stop at face k on an n-sided die"
    Rolling until face $k$ appears,

    $$
    \mathbb{E}[M]=k+\sum_{m=k+1}^{n}\frac{n-m+1}{n-m+2}.
    $$

    For $n=6,\;k=4$: $4+\tfrac{2}{3}+\tfrac{1}{2}=\tfrac{31}{6}$.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 3 of 4 |
    | Topic | Probability |
    | Difficulty | Medium |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | October 22, 2026 |
    | Source | [OpenQuant: Highest Roll EV](https://openquant.co/questions/highest-roll-ev) |

---

[Previous question](question-2.md) · [Day 1 overview](index.md) · [Next question](question-4.md)
