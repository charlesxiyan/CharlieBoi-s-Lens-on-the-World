# Day 6 · Four of a Kind

*Edited by Sophia*

**Probability · Combinatorics · Counting poker hands**

[Original question on OpenQuant](https://openquant.co/questions/four-of-a-kind)

## Problem

Five cards are dealt from a standard 52-card deck.
What is the probability that the hand is a **four of a kind**?

!!! tip "Core idea"
    Every four of a kind has the same shape, $\{A,A,A,A,B\}$: four cards of one rank
    plus one card of a different rank. Count the shapes, then divide by all 5-card hands.

## Short solution

**1. Choose the rank of the quad.** The four cards of rank $A$ are forced once $A$ is chosen,
and there are 13 ranks:

$$
\binom{13}{1}=13.
$$

**2. Choose the fifth card.** Any of the remaining $52-4=48$ cards works,
since none of them can share rank $A$:

$$
\binom{48}{1}=48.
$$

**3. Count the four-of-a-kind hands.**

$$
\#\{\text{four of a kind}\}=13\cdot48=624.
$$

**4. Divide by all hands.** A hand is an unordered set of 5 cards:

$$
\#\{\text{hands}\}=\binom{52}{5}=2{,}598{,}960.
$$

!!! success "Answer"
    $$\boxed{P=\frac{624}{2{,}598{,}960}=\frac{1}{4165}\approx0.024\%}$$

!!! warning "Watch out"
    Hands are **unordered**, so don't multiply by the $5!$ orderings of the cards,
    or by the 5 positions of the odd card. The numerator and the denominator
    must both count unordered sets.

??? note "Alternative check: choose the single card first"
    Read the shape as $\{A,B,B,B,B\}$ instead. The single card can be any of the 52 cards.
    The quad can then be any rank **except** the single's rank, since only 3 cards of
    that rank remain, leaving 12 choices:

    $$
    52\cdot12=624.
    $$

    The count is the same whichever part of the hand we choose first.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 2 of 4 |
    | Topic | Probability |
    | Difficulty | Easy |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | November 12, 2026 |
    | Source | [OpenQuant: Four of a Kind](https://openquant.co/questions/four-of-a-kind) |

---

[Previous question](question-1.md) · [Day 6 overview](index.md) · [Next question](question-3.md)