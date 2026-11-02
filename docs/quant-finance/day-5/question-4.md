# Day 5 · Fruit Basket

*Edited by Charles*

**Probability · Random permutations · Inclusion–exclusion**

[Original question on OpenQuant](https://openquant.co/questions/fruit-basket)

## Problem

A basket contains 10 apples, 20 oranges, and 30 peaches. Remove fruits uniformly
at random without replacement. Immediately after removing the final apple,
what is the probability that at least one orange and at least one peach remain?

!!! tip "Core idea"
    View removal as a random ordering. Compare the last apple with the last
    fruit of each other type, then apply inclusion–exclusion to the failure events.

## Short solution

**1. Define the failure events.** Let $A$ mean no oranges remain when the final
apple is removed, and let $B$ mean no peaches remain at that time.
The desired event is $A^c\cap B^c$, so

$$P(A^c\cap B^c)=1-P(A)-P(B)+P(A\cap B).$$

**2. Calculate $P(A)$.**

*Meaning: every orange is removed before the final apple. Among only apples
and oranges, the last fruit removed must therefore be an apple.*

Ignore peaches: removing them from the ordering does not change the relative
order of apples and oranges. Each of these 30 individual fruits is equally
likely to be last, and 10 of them are apples:

$$P(A)=\frac{10}{10+20}=\frac13.$$

**3. Calculate $P(B)$.**

*Meaning: every peach is removed before the final apple. Among only apples
and peaches, the last fruit removed must be an apple.*

Ignore oranges and use the same symmetry among 40 fruits:

$$P(B)=\frac{10}{10+30}=\frac14.$$

**4. Calculate $P(A\cap B)$.**

*Meaning: both oranges and peaches are exhausted when the final apple is
removed. Equivalently, the very last fruit in the entire basket is an apple.*

There are 10 apples among all 60 individual fruits, each equally likely to be last:

$$P(A\cap B)=\frac{10}{60}=\frac16.$$

**5. Combine the probabilities.** Subtract the two failure probabilities from
one, then add back their overlap because it was subtracted twice.

!!! success "Answer"
    $$\boxed{P=1-\frac13-\frac14+\frac16=\frac7{12}\approx58.33\%}$$

!!! warning "Watch out"
    The two failure events are not independent. Also, equal likelihood applies
    to individual fruits, not to the fruit types: apples and oranges are not
    equally likely to be the last fruit in their combined ordering.

??? note "Small example: why the last fruit determines A"
    With two apples (A) and three oranges (O), the order `O A O O A` satisfies
    the event: every orange is gone when the final apple is removed.
    The order `O A O A O` does not, because one orange remains.

    Inserting peaches anywhere in either sequence does not change that conclusion.

---

[Previous question](question-3.md) · [Day 5 overview](index.md)
