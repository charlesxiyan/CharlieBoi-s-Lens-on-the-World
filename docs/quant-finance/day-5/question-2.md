# Day 5 · How Many Heads

*Edited by Charles*

**Probability · Bayes’ theorem · Conditional expectation**

[Original question on OpenQuant](https://openquant.co/questions/how-many-heads)

## Problem

Choose one of three coins uniformly at random. Their probabilities of heads
are $1/3$, $2/3$, and $1$. The selected coin produces two heads.
Keeping the same coin, what is the expected number of heads in the next twelve flips?
Flips are independent conditional on the selected coin.

!!! tip "Core idea"
    Two observed heads make the coins with higher head probabilities more
    likely. Update the coin probabilities before calculating the expectation.

## Short solution

**1. Calculate the likelihood of two heads.** Let $H_2$ denote the observation.

| Coin | Heads probability | $P(H_2\mid\text{coin})$ |
| --- | --- | --- |
| A | $1/3$ | $1/9$ |
| B | $2/3$ | $4/9$ |
| C | $1$ | $1$ |

**2. Apply Bayes' theorem.** Equal prior probabilities cancel when normalizing:

$$P(A\mid H_2)=\frac{1/9}{1/9+4/9+1}=\frac1{14},$$

$$P(B\mid H_2)=\frac4{14},\qquad P(C\mid H_2)=\frac9{14}.$$

**3. Average the future head probability.**

$$E[p\mid H_2]=\frac1{14}\cdot\frac13+
\frac4{14}\cdot\frac23+\frac9{14}\cdot1=\frac67.$$

If $N$ counts heads in the next twelve flips, then $E[N\mid p,H_2]=12p$.
Taking the posterior average gives:

!!! success "Answer"
    $$\boxed{E[N\mid H_2]=12E[p\mid H_2]=\frac{72}{7}\approx10.286}$$

!!! warning "Watch out"
    Do not retain the original equal coin probabilities after observing two
    heads. The requested count excludes the two flips already observed.

??? note "Why multiplying by twelve is valid"
    Each future flip has conditional head probability $6/7$ given $H_2$.
    Linearity of expectation adds those twelve probabilities. It does not
    require the future flips to be independent after averaging over the
    unknown coin; the shared coin actually makes them dependent under that mixture.

---

[Previous question](question-1.md) · [Day 5 overview](index.md) · [Next question](question-3.md)
