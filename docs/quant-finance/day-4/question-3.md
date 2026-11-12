# Day 4 · Painted Cube

*Edited by Charles and Sophia*

**Combinatorics · Spatial reasoning**

[Original question on OpenQuant](https://openquant.co/questions/painted-cube)

## Problem

A solid $10\times10\times10$ cube is assembled from unit cubes. Paint coats
its entire outer surface without entering gaps between the small cubes.
How many unit cubes remain completely unpainted?

!!! tip "Core idea"
    A small cube stays unpainted exactly when it is strictly inside the large
    cube. Remove the outermost layer from both ends of every axis.

## Short solution

**1. Identify allowed positions along one axis.** Number positions from $1$
to $10$. A cube at position $1$ or $10$ touches the exterior in that direction.
Only positions $2,3,\ldots,9$ can belong to the unpainted interior: eight choices.

**2. Apply the restriction on all three axes.** Every unpainted cube must have
an interior position in the horizontal, vertical, and depth directions.
The interior is an $8\times8\times8$ cube.

!!! success "Answer"
    $$\boxed{(10-2)^3=8^3=512\text{ unpainted cubes}}$$

!!! warning "Watch out"
    Subtracting $6\cdot10^2$ from $10^3$ double-counts edge cubes and triple-counts
    corner cubes. Counting the interior directly avoids overlapping faces.

??? note "Check by classifying the painted cubes"
    There are eight corners with three painted faces,
    $12(10-2)=96$ edge cubes with two painted faces, and
    $6(10-2)^2=384$ face-interior cubes with one painted face.

    $$8+96+384=488,\qquad 1000-488=512.$$

    More generally, an $n\times n\times n$ solid cube has
    $\max(n-2,0)^3$ completely unpainted unit cubes.

---

[Previous question](question-2.md) · [Day 4 overview](index.md) · [Next question](question-4.md)
