# Day 5 · Three Rings

*Edited by Charles and Sophia*

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

**3. Count the valid assignments for $3n$ arrows.** Assign the arrows to regions one region at a time:

- choose which $n$ of the $3n$ arrows land in the inner disk: $\binom{3n}{n}$ ways;
- choose which $n$ of the remaining $2n$ land in the middle ring: $\binom{2n}{n}$ ways;
- the last $n$ arrows all land in the outer ring: $\binom{n}{n}=1$ way.

$$
\#\{\text{valid assignments}\}=\binom{3n}{n}\binom{2n}{n}\binom{n}{n}.
$$

**4. Find the probability of one assignment.** In any fixed valid assignment, $n$ arrows hit each region.
By independence, every such assignment has the same probability:

$$
p_1^{\,n}\,p_2^{\,n}\,p_3^{\,n}=(p_1p_2p_3)^n
=\left(\frac19\cdot\frac13\cdot\frac59\right)^n
=\left(\frac{15}{729}\right)^n.
$$

**5. Multiply count by probability.**

!!! success "Part 2"
    $$\boxed{P=\binom{3n}{n}\binom{2n}{n}\binom{n}{n}\left(\frac{15}{729}\right)^n}$$

**6. Check small cases.**

| $n$ | Count $\binom{3n}{n}\binom{2n}{n}\binom{n}{n}$ | $\left(\frac{15}{729}\right)^n$ | $P$ |
| --- | --- | --- | --- |
| 1 | $3\cdot2\cdot1=6$ | $0.02058$ | $\tfrac{10}{81}\approx12.35\%$ |
| 2 | $15\cdot6\cdot1=90$ | $0.0004233$ | $\approx3.81\%$ |
| 3 | $84\cdot20\cdot1=1680$ | $0.000008711$ | $\approx1.46\%$ |

The case $n=1$ reproduces Part 1 exactly.

!!! warning "Watch out"
    Equal radial widths do not imply equal areas. Multiplying the probabilities
    without the arrangement factor counts only one order.

??? note "Same count, multinomial form"
    The product of binomials collapses to the multinomial coefficient:

    $$
    \binom{3n}{n}\binom{2n}{n}\binom{n}{n}
    =\frac{(3n)!}{n!\,(2n)!}\cdot\frac{(2n)!}{n!\,n!}\cdot 1
    =\frac{(3n)!}{(n!)^3}.
    $$

    So the answer is the multinomial probability
    $\dfrac{(3n)!}{(n!)^3}\,p_1^{\,n}p_2^{\,n}p_3^{\,n}$.
    Because $P$ shrinks as $n$ grows, seeing an **exactly** even split becomes
    increasingly unlikely, even though the expected counts are uneven anyway
    ($p_3=5/9$ dominates).

---

[Day 5 overview](index.md) · [Next question](question-2.md)
