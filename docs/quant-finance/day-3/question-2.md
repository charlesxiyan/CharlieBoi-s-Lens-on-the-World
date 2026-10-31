# Day 3 · Ascending Dice Rolls

*Edited by Charles*

**Probability · Counting · Strict ordering**

[Original question on OpenQuant](https://openquant.co/questions/ascending-dice-rolls)

## Problem

Roll a fair six-sided die three times independently. Find the probability that
its results satisfy $D_1<D_2<D_3$.

!!! tip "Core idea"
    Choose three distinct faces. Each choice has exactly one increasing order.

## Short solution

**1. Count all outcomes.** Order matters: $(1,2,3)$ and $(3,2,1)$ are different
outcomes. Independence gives $6^3=216$ equally likely triples.

**2. Count increasing triples.** Choose any three faces from the six available:

$$
\binom63=\frac{6\cdot5\cdot4}{3\cdot2\cdot1}=20.
$$

For example, the set $\{2,4,6\}$ contributes only $(2,4,6)$.
There is no additional permutation factor because the required order is fixed.

**3. Divide favorable outcomes by all outcomes.**

$$
P(D_1<D_2<D_3)=\frac{\binom63}{6^3}=\frac{20}{216}.
$$

!!! success "Answer"
    $$\boxed{\frac5{54}\approx9.26\%}$$

!!! warning "Watch out"
    The answer is not $1/3!$. That is the probability of increasing order
    **conditional on all three rolls being distinct**. Ties are possible here.

??? note "Alternative check: first exclude ties"
    The probability of three distinct results is

    $$
    1\cdot\frac56\cdot\frac46=\frac59.
    $$

    Conditional on distinct results, the six orderings are equally likely.
    Thus $(5/9)(1/6)=5/54$.

---

[Previous question](question-1.md) · [Day 3 overview](index.md) · [Next question](question-3.md)
