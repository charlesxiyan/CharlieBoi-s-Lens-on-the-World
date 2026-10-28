# Day 2 · First Steps

*Edited by Charles*

**Probability · Markov chains & stationary distributions**

[Original question on OpenQuant](https://openquant.co/questions/first-steps)

## Problem

A baby starts at a couch. At each time step, it moves one step away with
probability $0.2$, stays put with probability $0.5$, or moves one step back
with probability $0.3$. At the couch, a backward move becomes a stay:
$P(0\to0)=0.8$ and $P(0\to1)=0.2$.

In the long run, what fraction of time does the baby spend at the couch?

!!! tip "Core idea"
    Write an equation for every state's incoming probability. Start at the couch
    to express each unknown in terms of $\pi_0$, then use the fact that all
    probabilities sum to one to determine $\pi_0$.

## Worked solution

### 1. Define the unknowns and write the equations

Let $\pi_n$ be the long-run probability that the baby is $n$ steps from the
couch. In a stationary distribution, the probability of being in each state
is unchanged after one more step.

To arrive at state zero, the baby must stay at zero or move back from one:

$$
\pi_0=0.8\pi_0+0.3\pi_1.
$$

For any state $n\ge1$, it can arrive from $n-1$, stay at $n$, or move back
from $n+1$. Therefore the complete system is

$$
\begin{aligned}
\pi_0 &= 0.8\pi_0+0.3\pi_1,\\
\pi_1 &= 0.2\pi_0+0.5\pi_1+0.3\pi_2,\\
\pi_2 &= 0.2\pi_1+0.5\pi_2+0.3\pi_3,\\
&\ \vdots\\
\pi_n &= 0.2\pi_{n-1}+0.5\pi_n+0.3\pi_{n+1},\qquad n\ge1,
\end{aligned}
$$

together with $\sum_{n=0}^{\infty}\pi_n=1$ and $\pi_n\ge0$.

### 2. Solve for π₁ in terms of π₀

The first equation gives

$$
\pi_0-0.8\pi_0=0.3\pi_1
\quad\Longrightarrow\quad
\pi_1=\frac{0.2}{0.3}\pi_0=\frac23\pi_0.
$$

This is a relation, not yet a numerical value. We will determine $\pi_0$
using normalization after finding the remaining probabilities.

### 3. Use the state-one equation to find π₂

Rearrange the second equation and substitute $\pi_1=\frac23\pi_0$:

$$
\begin{aligned}
0.3\pi_2
&=0.5\pi_1-0.2\pi_0\\
&=\left(\frac12\cdot\frac23-\frac15\right)\pi_0\\
&=\left(\frac13-\frac15\right)\pi_0
=\frac{2}{15}\pi_0.
\end{aligned}
$$

Dividing by $0.3=3/10$ gives

$$
\pi_2=\frac{2}{15}\cdot\frac{10}{3}\pi_0
=\frac49\pi_0.
$$

### 4. Use the state-two equation to find π₃

Similarly,

$$
\begin{aligned}
0.3\pi_3
&=0.5\pi_2-0.2\pi_1\\
&=\left(\frac12\cdot\frac49-\frac15\cdot\frac23\right)\pi_0\\
&=\left(\frac29-\frac{2}{15}\right)\pi_0
=\frac{4}{45}\pi_0.
\end{aligned}
$$

Hence

$$
\pi_3=\frac{4}{45}\cdot\frac{10}{3}\pi_0
=\frac{8}{27}\pi_0.
$$

### 5. Establish the pattern for every state

So far,

$$
\pi_1=\frac23\pi_0,\qquad
\pi_2=\left(\frac23\right)^2\pi_0,\qquad
\pi_3=\left(\frac23\right)^3\pi_0.
$$

To prove the pattern continues, rearrange the general state equation:

$$
0.3\pi_{n+1}=0.5\pi_n-0.2\pi_{n-1}.
$$

If $\pi_n=\frac23\pi_{n-1}$, then $\pi_{n-1}=\frac32\pi_n$, giving

$$
0.3\pi_{n+1}
=\left(0.5-0.2\cdot\frac32\right)\pi_n
=0.2\pi_n.
$$

Thus $\pi_{n+1}=\frac23\pi_n$. Since the relation holds at the boundary,
it holds at every subsequent state by induction:

$$
\pi_n=\left(\frac23\right)^n\pi_0,\qquad n\ge0.
$$

### 6. Normalize to determine π₀

All state probabilities must sum to one:

$$
\begin{aligned}
1&=\pi_0+\pi_1+\pi_2+\cdots\\
&=\pi_0\left[1+\frac23+\left(\frac23\right)^2+\cdots\right]\\
&=\pi_0\frac{1}{1-2/3}
=3\pi_0.
\end{aligned}
$$

Therefore $\pi_0=1/3$, and the full stationary distribution is

$$
\pi_n=\frac13\left(\frac23\right)^n.
$$

For example, $\pi_1=2/9$, $\pi_2=4/27$, and $\pi_3=8/81$.

!!! success "Answer"
    $$\boxed{\pi_0=\frac13\approx33.33\%}$$

    The baby spends one third of its time at the couch in the long run.

!!! warning "Watch out"
    $0.8$ is the chance of **staying at the couch for the next step**, given that
    the baby is already there. It is not the long-run fraction of time there.
    Keeping the stay-put steps matters because the question measures all time steps.

??? note "Why a stationary distribution exists"
    Away from the boundary, the backward probability $0.3$ exceeds the forward
    probability $0.2$, so the process drifts toward the couch. The geometric
    distribution above is summable. The chain is irreducible and has self-loops,
    so its long-run time proportions equal these stationary probabilities.

??? note "Quick check"
    The expected change in distance is $+0.2$ at state zero and $-0.1$ elsewhere.
    Under the stationary distribution,

    $$
    0=0.2\pi_0-0.1(1-\pi_0)
    \quad\Longrightarrow\quad\pi_0=\frac13.
    $$

---

[Previous question](question-2.md) · [Day 2 overview](index.md) · [Next question](question-4.md)
