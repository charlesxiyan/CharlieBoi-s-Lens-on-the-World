# Day 4 · Ten Topping Sundae

*Edited by Charles*

**Combinatorics · Subsets · Binomial coefficients**

[Original question on OpenQuant](https://openquant.co/questions/ten-topping-sundae)

## Problem

Choose at least one of ten distinct ice-cream toppings. Each topping is either
included or excluded; order and quantity do not distinguish choices.
How many different selections are possible?

!!! tip "Core idea"
    Count every subset by making a yes/no decision for each topping, then
    remove the empty selection.

## Short solution

**1. Give each topping two choices.** For each of the ten toppings, decide
whether to include it. These unrestricted decisions produce

$$2\times2\times\cdots\times2=2^{10}$$

distinct subsets, including the choice of no toppings.

**2. Remove the empty subset.** There is exactly one all-no selection.

!!! success "Answer"
    $$\boxed{2^{10}-1=1{,}023}$$

**3. Connect this to the sum of combinations.** Alternatively, group selections
by how many toppings they contain:

$$\sum_{k=1}^{10}\binom{10}{k}=2^{10}-1.$$

The general identity

$$\sum_{k=0}^{n}\binom nk=2^n$$

counts the same subsets in two ways. The left side groups them by size, while
the right side counts all binary include/exclude decisions. Every subset has
exactly one size, so no selection is omitted or counted twice.

!!! warning "Watch out"
    $\binom{10}{k}$ counts selections of a fixed size $k$. This question permits
    every nonzero size. Do not multiply by a permutation factor: topping order
    does not create a new selection.

??? note "Small example: three toppings"
    With toppings A, B, and C, the nonempty selections are A, B, C, AB, AC, BC,
    and ABC. There are $\binom31+\binom32+\binom33=3+3+1=7=2^3-1$.

---

[Previous question](question-1.md) · [Day 4 overview](index.md) · [Next question](question-3.md)
