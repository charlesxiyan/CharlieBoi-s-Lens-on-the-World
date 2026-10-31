# Day 3 · Portfolio Manager

*Edited by Charles*

**Combinatorics · Stars and bars · Minimum allocations**

[Original question on OpenQuant](https://openquant.co/questions/portfolio-manager)

## Problem

Allocate a 75-million-dollar fund among five distinct asset classes in whole
million-dollar units, investing the entire fund.

1. How many allocations are possible if a class may receive zero?
2. How many remain if every class must receive at least five million dollars?

!!! tip "Core idea"
    Count nonnegative integer allocations. For minimum requirements, reserve
    the mandatory amounts first and distribute the remainder.

## Short solution

**1. Allow zero allocations.** In million-dollar units,

$$x_1+x_2+x_3+x_4+x_5=75,\qquad x_i\ge0.$$

Represent the money by 75 identical stars and separate the five labeled classes
with four bars. Selecting the bar positions gives

$$N_1=\binom{75+5-1}{5-1}=\binom{79}{4}=1{,}502{,}501.$$

**2. Reserve the minimum.** Set $z_i=x_i-5\ge0$. The five minimum allocations
use $25$ units, leaving

$$z_1+z_2+z_3+z_4+z_5=50.$$

Apply the same count:

$$N_2=\binom{50+5-1}{5-1}=\binom{54}{4}=316{,}251.$$

!!! success "Answers"
    $$\boxed{N_1=1{,}502{,}501,\qquad N_2=316{,}251}$$

!!! warning "Watch out"
    Investment units are indistinguishable; asset classes are labeled.
    Adjacent bars correctly allow an allocation of zero. Part 1 does not
    require a positive investment in every class.

??? note "Small example: two units across three classes"
    The six allocations are $(2,0,0)$, $(0,2,0)$, $(0,0,2)$, $(1,1,0)$,
    $(1,0,1)$, and $(0,1,1)$. This matches $\binom42=6$.

---

[Previous question](question-3.md) · [Day 3 overview](index.md) · [Next question](question-5.md)
