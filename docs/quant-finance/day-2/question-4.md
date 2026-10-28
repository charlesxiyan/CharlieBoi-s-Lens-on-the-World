# Day 2 · Dice Game 1

*Edited by Charles*

**Probability · First-step recursion**

[Original question on OpenQuant](https://openquant.co/questions/dice-game-1)

## Problem

A and B alternate turns, rolling two independent fair six-sided dice each turn.
A goes first and wins on a sum of $6$; B wins on a sum of $7$.
If neither wins, play continues. What is the probability that A wins?

!!! tip "Core idea"
    If both players miss, the game returns to exactly its starting situation:
    A is about to roll again.

## Short solution

**1. Find the chance of winning on one turn.** There are 36 ordered dice outcomes:

| Player | Target sum | Winning outcomes | Win probability per turn |
| --- | --- | --- | --- |
| A | $6$ | $(1,5),(2,4),(3,3),(4,2),(5,1)$ | $a=5/36$ |
| B | $7$ | $(1,6),(2,5),(3,4),(4,3),(5,2),(6,1)$ | $b=6/36$ |

**2. Write a restart equation.** Let $p$ be A's eventual win probability.
A either wins immediately, or both miss and the game restarts:

$$
p=a+(1-a)(1-b)p
=\frac5{36}+\frac{31}{36}\cdot\frac{30}{36}\,p.
$$

**3. Solve for $p$.**

$$
p=\frac{a}{1-(1-a)(1-b)}
=\frac{5/36}{1-(31/36)(30/36)}.
$$

!!! success "Answer"
    $$\boxed{p=\frac{30}{61}\approx49.18\%}$$

!!! warning "Watch out"
    Do not use $5/(5+6)$. That ignores turn order: B only gets a chance to win
    after A misses. A has the first-move advantage, but B's higher per-turn
    success probability still leaves A slightly below $50\%$.

??? note "Quick check: compare wins in one round"
    A wins a round with probability $a=30/216$.
    B wins a round with probability $(1-a)b=31/216$.
    Conditional on a round ending the game, A's share is

    $$
    \frac{30/216}{30/216+31/216}=\frac{30}{61}.
    $$

---

[Previous question](question-3.md) · [Day 2 overview](index.md)
