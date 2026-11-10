# Day 5 · How Many Heads

*Edited by Charles and Sophia*

**Probability · Bayes’ theorem · Conditional expectation**

[Original question on OpenQuant](https://openquant.co/questions/how-many-heads)

## Problem

Choose one of three coins uniformly at random. Their probabilities of heads
are $1/3$, $2/3$, and $1$. Flips are independent conditional on the selected coin.

0. **Warm-up.** Before any flips are observed, what is the expected number of heads in twelve flips?
1. The selected coin produces two heads. Keeping the same coin, what is the expected
   number of heads in the **next** twelve flips?

!!! tip "Core idea"
    The expected number of heads is $12$ times the **average** head probability,
    weighted by how likely each coin is. Observing two heads does not change the coins;
    it changes the weights from the prior $\left(\tfrac13,\tfrac13,\tfrac13\right)$ to a posterior
    that favors coins with higher head probabilities.

## Short solution

### Warm-up · No information yet

**1. Average the head probability over the prior.** Each coin is chosen with probability $1/3$:

$$
E[p]=\frac13\cdot\frac13+\frac13\cdot\frac23+\frac13\cdot1=\frac23.
$$

**2. Scale to twelve flips.** Let $N$ count heads in twelve flips. Given the coin,
$E[N\mid p]=12p$, so

$$
E[N]=12\,E[p]=12\cdot\frac23=8.
$$

The two observed heads in the main question are evidence about **which** coin we hold.
Bayes' theorem uses that evidence to update the weights $\tfrac13,\tfrac13,\tfrac13$
before we repeat exactly the same calculation.

### Main question · After two heads

**1. Calculate the likelihood of two heads.** Let $H_2$ denote the observation.

| Coin | Heads probability | $P(H_2\mid\text{coin})$ |
| --- | --- | --- |
| A | $1/3$ | $1/9$ |
| B | $2/3$ | $4/9$ |
| C | $1$ | $1$ |

**2. Apply Bayes' theorem.** Equal prior probabilities cancel when normalizing:

$$P(A\mid H_2)=\frac{1/9}{1/9+4/9+1}=\frac1{14},$$

$$P(B\mid H_2)=\frac4{14},\qquad P(C\mid H_2)=\frac9{14}.$$

The update at a glance:

| Coin | $p$ | Prior weight | Posterior weight |
| --- | --- | --- | --- |
| A | $1/3$ | $1/3\approx0.33$ | $1/14\approx0.07$ |
| B | $2/3$ | $1/3\approx0.33$ | $4/14\approx0.29$ |
| C | $1$ | $1/3\approx0.33$ | $9/14\approx0.64$ |

**3. Average the future head probability with the new weights.**

$$E[p\mid H_2]=\frac1{14}\cdot\frac13+
\frac4{14}\cdot\frac23+\frac9{14}\cdot1=\frac67.$$

If $N$ counts heads in the next twelve flips, then $E[N\mid p,H_2]=12p$.
Taking the posterior average gives:

!!! success "Answer"
    $$\boxed{\text{Warm-up: }E[N]=8,\qquad E[N\mid H_2]=12E[p\mid H_2]=\frac{72}{7}\approx10.286}$$

!!! warning "Watch out"
    Do not retain the original equal coin probabilities after observing two
    heads; doing so gives the warm-up answer $8$ instead of $72/7$.
    The requested count also excludes the two flips already observed.

??? note "Why multiplying by twelve is valid"
    Each future flip has conditional head probability $6/7$ given $H_2$.
    Linearity of expectation adds those twelve probabilities. It does not
    require the future flips to be independent after averaging over the
    unknown coin; the shared coin actually makes them dependent under that mixture.

---

[Previous question](question-1.md) · [Day 5 overview](index.md) · [Next question](question-3.md)
