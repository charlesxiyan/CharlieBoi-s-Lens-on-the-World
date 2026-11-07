# Day 4 · King of Hearts

*Edited by Charles and Sophia*

**Probability · Conditional probability · Counting arrangements**

[Original question on OpenQuant](https://openquant.co/questions/king-of-hearts)

## Problem

A standard 52-card deck is uniformly shuffled. The first ace appears at position
14. Given **only this information**, what is the probability that the card at
position 15 is the king of hearts?

!!! tip "Core idea"
    Every ordering of the deck is equally likely, so a conditional probability is a
    ratio of counts: (orderings that satisfy the condition **and** put K♥ at 15)
    over (orderings that satisfy the condition).

## Short solution

**1. Count the valid orderings (the denominator).** "First ace at position 14" means
cards 1–13 are non-aces and card 14 is an ace. Fill the deck left to right:

| Positions | Choice | Count |
| --- | --- | --- |
| 1–13 | ordered choice of 13 of the 48 non-aces | $\binom{48}{13}\,13!$ |
| 14 | one of the 4 aces | $\binom{4}{1}$ |
| 15–52 | the remaining 38 cards in any order | $38!$ |

$$
\#\{\text{valid}\}=\binom{48}{13}\,13!\cdot\binom{4}{1}\cdot 38!.
$$

**2. Count the valid orderings with K♥ at position 15 (the numerator).**
K♥ must not appear in positions 1–13, so those 13 cards come from the
47 non-aces other than K♥:

| Positions | Choice | Count |
| --- | --- | --- |
| 1–13 | ordered choice of 13 of the 47 non-ace, non-K♥ cards | $\binom{47}{13}\,13!$ |
| 14 | one of the 4 aces | $\binom{4}{1}$ |
| 15 | K♥ | $\binom{1}{1}$ |
| 16–52 | the remaining 37 cards in any order | $37!$ |

$$
\#\{\text{valid},\ \text{K♥ at 15}\}=\binom{47}{13}\,13!\cdot\binom{4}{1}\cdot\binom{1}{1}\cdot 37!.
$$

**3. Take the ratio.** The $13!$ and $\binom41$ cancel, leaving two simple ratios:

$$
P=\frac{\binom{47}{13}}{\binom{48}{13}}\cdot\frac{37!}{38!}
=\frac{35}{48}\cdot\frac{1}{38}
=\frac{35}{1824}.
$$

!!! success "Answer"
    $$\boxed{\frac{35}{1824}\approx 1.92\%}$$

!!! warning "Watch out"
    The tempting answer is $1/38$, treating K♥ as certainly still in the deck.
    But the condition says nothing about **which** non-aces filled positions 1–13,
    so K♥ may already be gone. Excluding it from those 13 slots is exactly
    what produces the extra factor $35/48$.

??? note "Why the two ratios simplify so cleanly"
    $$
    \frac{\binom{47}{13}}{\binom{48}{13}}
    =\frac{47!/(13!\,34!)}{48!/(13!\,35!)}
    =\frac{35}{48},
    \qquad
    \frac{37!}{38!}=\frac{1}{38}.
    $$

    Each ratio has a meaning. $35/48$ is the probability that K♥ is **not** among the
    first 13 non-aces, and $1/38$ is the probability that it then lands
    **first** among the 38 remaining cards.

---

[Previous question](question-4.md) · [Day 4 overview](index.md)
