# Day 7 · Biased Coin Sequence

*Edited by Charles and Sophia*

**Probability · First-step analysis · Pattern waiting times**

[Original question on OpenQuant](https://openquant.co/questions/biased-coin-sequence)

## Problem

A coin lands heads with probability $\tfrac23$ and tails with probability $\tfrac13$.
You flip it until you see a **tails immediately followed by heads** ($TH$).
What is the expected number of flips?

!!! tip "Core idea"
    Condition on the first flip. An $H$ makes no progress, so we are back where we started.
    A $T$ means we only need one $H$ next, and any further $T$ keeps us in the same position,
    so the remaining wait is a simple geometric wait for heads.

## Short solution

**1. Set up the starting state.** Let $E[TH]$ be the expected number of flips needed
to see $TH$, starting from **no flips at all**.

**2. First flip is $H$ (probability $\tfrac23$): no progress.**
A leading $H$ can't be part of a $TH$, so after this flip we are in exactly the same
situation as at the start. We have used 1 flip and still expect $E[TH]$ more:

$$
\text{expected total}=1+E[TH].
$$

**3. First flip is $T$ (probability $\tfrac13$): wait for one $H$.**
Now we finish on the next $H$. Another $T$ doesn't hurt: the last flip is still a $T$,
so we are in the same "just need an $H$" position. The remaining wait is therefore
the waiting time for a single heads, which is geometric with $p=\tfrac23$.
Its expectation is the **reciprocal of the probability**:

$$
E[\text{flips until }H]=\frac{1}{p}=\frac{1}{2/3}=\frac32,
\qquad
\text{expected total}=1+\frac32.
$$

**4. Combine the two cases (first-step analysis).**

$$
E[TH]=\frac23\big(E[TH]+1\big)+\frac13\left(\frac32+1\right).
$$

**5. Solve.**

$$
E[TH]=\frac23E[TH]+\frac23+\frac56
\quad\Longrightarrow\quad
\frac13E[TH]=\frac32
\quad\Longrightarrow\quad
E[TH]=\frac92.
$$

!!! success "Answer"
    $$\boxed{E[TH]=\frac92=4.5\text{ flips}}$$

!!! warning "Watch out"
    The shortcut "after a $T$, a $T$ keeps us in the same position" works because
    $TH$ cannot overlap itself. For a pattern like $HH$, a $T$ in the middle
    **destroys** progress and sends us back to the start, so that pattern needs
    a different recursion.

??? note "Shortcut: two geometric waits"
    The same reasoning splits the whole wait into two independent pieces:
    first wait for a $T$, then wait for an $H$.

    $$
    E[TH]=\underbrace{\frac{1}{1/3}}_{\text{wait for }T}+\underbrace{\frac{1}{2/3}}_{\text{wait for }H}
    =3+\frac32=\frac92.
    $$

    For a coin with $P(H)=p$ and $P(T)=q=1-p$, this gives
    $E[TH]=\dfrac1q+\dfrac1p=\dfrac{1}{pq}$.
    Check: $\tfrac{1}{(2/3)(1/3)}=\tfrac92$.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 3 of 4 |
    | Topic | Probability |
    | Difficulty | Medium |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | November 13, 2026 |
    | Source | [OpenQuant: Biased Coin Sequence](https://openquant.co/questions/biased-coin-sequence) |

---

[Previous question](question-2.md) · [Day 7 overview](index.md) · [Next question](question-4.md)