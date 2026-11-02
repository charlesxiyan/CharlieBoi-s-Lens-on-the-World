# Day 5 · Three Rings

*Edited by Charles*

**Probability · Area · Multinomial distribution**

[Original question on OpenQuant](https://openquant.co/questions/three-rings)

## Problem

A circular target has three regions: an inner disk of radius 4 inches, a middle
ring extending to radius 8, and an outer ring extending to radius 12.
Arrows land independently and uniformly over the target's area.

1. With three arrows, what is the probability of exactly one hit in each region?
2. With $3n$ arrows, what is the probability of exactly $n$ hits in each region?

!!! tip "Core idea"
    Hit probabilities depend on area, not radial width. Then count the possible
    orders of the hits.

## Short solution

**1. Calculate the three probabilities.** The total area is $144\pi$:

$$p_1=\frac{4^2}{12^2}=\frac19,\qquad
p_2=\frac{8^2-4^2}{12^2}=\frac13,\qquad
p_3=\frac{12^2-8^2}{12^2}=\frac59.$$

**2. Include every ordering of three arrows.** One particular order, inner then
middle then outer, has probability $p_1p_2p_3$. There are $3!=6$ such orders.

!!! success "Part 1"
    $$\boxed{P=3!\cdot\frac19\cdot\frac13\cdot\frac59
    =\frac{10}{81}\approx12.35\%}$$

**3. Generalize to $3n$ arrows.** The number of arrangements containing $n$
hits of each type is $(3n)!/(n!)^3$. Every such arrangement has the same probability.

!!! success "Part 2"
    $$\boxed{P=\frac{(3n)!}{(n!)^3}
    \left(\frac19\right)^n\left(\frac13\right)^n\left(\frac59\right)^n}$$

!!! warning "Watch out"
    Equal radial widths do not imply equal areas. Multiplying the probabilities
    without the arrangement factor counts only one order.

---

[Day 5 overview](index.md) · [Next question](question-2.md)
