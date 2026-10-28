# Day 1 · Dot Placement

*Edited by Charles*

**Probability · Sequential counting**

[Original question on OpenQuant](https://openquant.co/questions/dot-placement)

## Problem

Place three dots independently and uniformly along the perimeter of a regular
octagon. What is the probability that they lie on **three distinct edges**?

!!! tip "Core idea"
    Each dot has eight equally likely edges. Once an edge is used,
    the next dot must avoid it.

## Short solution

| Dot | Available edges | Conditional probability |
| --- | --- | --- |
| First | Any of the 8 edges | $8/8$ |
| Second | Avoid the first edge | $7/8$ |
| Third | Avoid the two already used | $6/8$ |

Multiply along this sequence:

$$
P(\text{all distinct})
=\frac{8}{8}\cdot\frac{7}{8}\cdot\frac{6}{8}.
$$

!!! success "Answer"
    $$\boxed{P=\frac{21}{32}=65.625\%}$$

!!! warning "Watch out"
    Placements are independent, so a later dot can land on an already used edge.
    We count only outcomes with distinct edges. Exact vertices have probability
    zero under uniform placement along the perimeter.

??? note "Alternative counting and generalization"
    There are $8^3$ ordered edge assignments and $8\cdot7\cdot6$ favorable ones:

    $$
    P=\frac{8\cdot7\cdot6}{8^3}=\frac{21}{32}.
    $$

    For $k$ independent dots and $n$ equally likely edges,

    $$
    P(\text{all distinct})=\frac{n!}{(n-k)!\,n^k},\qquad 1\le k\le n.
    $$

    If $k>n$, the probability is zero.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 4 of 4 |
    | Topic | Probability |
    | Difficulty | Easy |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | October 22, 2026 |
    | Source | [OpenQuant: Dot Placement](https://openquant.co/questions/dot-placement) |

---

[Previous question](question-3.md) · [Day 1 overview](index.md)
