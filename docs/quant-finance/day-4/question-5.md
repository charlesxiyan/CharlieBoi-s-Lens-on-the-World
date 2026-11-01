# Day 4 · King of Hearts

*Edited by Charles*

**Probability · Conditional probability · Sampling without replacement**

[Original question on OpenQuant](https://openquant.co/questions/king-of-hearts)

## Problem

A standard 52-card deck is uniformly shuffled. The first ace appears at position
14. Given **only this information**, what is the probability that the card at
position 15 is the king of hearts?

!!! tip "Core idea"
    The king of hearts might already have appeared among the first 13 cards.
    Account for its survival before using the 38 cards still unrevealed.

## Short solution

**1. Condition on the first ace being card 14.** Call this event $A$.
The first 13 cards are non-aces, card 14 is an ace, and the remaining 38 cards
contain three aces and 35 non-aces.

**2. Find the probability that the king of hearts is still unseen.** There are
48 non-aces, and conditional on $A$ they remain symmetric. The first 13 positions
contain a uniformly chosen subset of 13 of them. Therefore,

$$P(\text{king of hearts remains}\mid A)=\frac{48-13}{48}=\frac{35}{48}.$$

**3. Condition on the king still being in the deck.** The remaining 38 cards are
in uniformly random order, so its probability of being next is $1/38$.
Multiply the two conditional factors:

$$
P(\text{card 15 is king of hearts}\mid A)
=\frac{35}{48}\cdot\frac1{38}.
$$

!!! success "Answer"
    $$\boxed{\frac{35}{1824}\approx1.9189\%}$$

!!! warning "Watch out"
    $1/38$ is correct only with the additional information that the king of
    hearts has not appeared. If you saw it among the first 13 cards, the answer
    would be zero. The question here conditions only on the position of the
    first ace, so it averages over both possibilities.

??? note "Alternative check: identify the next card as a non-ace first"
    Given $A$, the next card is a non-ace with probability $35/38$.
    Conditional on that event, all 48 named non-aces are symmetric when the
    earlier non-ace identities are unknown. Thus the probability it is the
    king of hearts is

    $$\frac{35}{38}\cdot\frac1{48}=\frac{35}{1824}.$$

    The two factorizations condition in different orders but give the same result.

---

[Previous question](question-4.md) · [Day 4 overview](index.md)
