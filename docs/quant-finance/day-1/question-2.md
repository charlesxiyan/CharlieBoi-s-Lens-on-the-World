# Day 1 · Race to Ace

*Edited by Charles*

**Probability · Symmetry & expectation**

[Original question on OpenQuant](https://openquant.co/questions/race-to-ace)

## Problem

Draw cards from a uniformly shuffled standard 52-card deck, without replacement.
How many draws do you expect to need to reach the first ace, **including the ace**?

!!! tip "Core idea"
    The four aces split the 48 other cards into five gaps. By symmetry,
    each gap has the same expected number of cards.

## Short solution

**1. Split the deck into gaps.** Let $G_i$ be the number of non-ace cards in gap $i$:

$$
G_1\;|\;A\;|\;G_2\;|\;A\;|\;G_3\;|\;A\;|\;G_4\;|\;A\;|\;G_5.
$$

**2. Use symmetry.** The gaps sum to 48, and their expectations are equal:

$$
\sum_{i=1}^{5}G_i=48
\quad\Longrightarrow\quad
\mathbb{E}[G_1]=\frac{48}{5}.
$$

**3. Count the ace itself.** The first ace arrives after the first gap:

$$
\mathbb{E}[X]=\mathbb{E}[G_1]+1=\frac{48}{5}+1.
$$

!!! success "Answer"
    $$\boxed{\mathbb{E}[X]=\frac{53}{5}=10.6\text{ draws}}$$

!!! warning "Watch out"
    $9.6$ counts only the cards **before** the first ace. The question includes
    the ace, so add one.

??? note "Generalization: the r-th special card"
    For a uniformly shuffled deck of $N$ cards with $K$ special cards,
    the expected position of the $r$-th special card is

    $$
    \mathbb{E}[X_r]
    =r\frac{N-K}{K+1}+r
    =\frac{r(N+1)}{K+1},\qquad 1\le r\le K.
    $$

    For the second ace, $\mathbb{E}[X_2]=2\cdot53/5=21.2$.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 2 of 4 |
    | Topic | Probability |
    | Difficulty | Medium |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | October 22, 2026 |
    | Source | [OpenQuant: Race to Ace](https://openquant.co/questions/race-to-ace) |

---

[Previous question](question-1.md) · [Day 1 overview](index.md) · [Next question](question-3.md)
