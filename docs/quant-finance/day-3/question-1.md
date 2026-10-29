---
title: Doubling Your Accuracy — Optimal Measurement Design and OLS
description: Sum-and-difference measurements, OLS covariance, and the role of orthogonality in reducing estimation variance.
tags:
  - Statistics
  - Quant Research
  - OLS
  - Experimental Design
  - Variance Reduction
  - Linear Algebra
---

# Day 3 · Doubling Your Accuracy — Optimal Measurement Design and OLS

*Edited by Charles*

**Statistics · OLS · Experimental Design · Variance Reduction**

[Original problem](https://bt.slray.com/q/2026-10-28_am)

## Problem statement

Two rods have fixed, unknown lengths $a$ and $b$. A measurement of a signed
linear combination of their lengths gives

$$
y=\ell+\varepsilon,\qquad
\mathbb E[\varepsilon]=0,\qquad
\operatorname{Var}(\varepsilon)=\sigma^2>0.
$$

There are exactly **two measurements**. Their errors are independent, have the
same variance, and do not become more variable when the measured length grows.
Find unbiased estimators of both lengths that improve on measuring each rod
separately.

For the optimality proof, a measurement is restricted to

$$
\ell=x_1a+x_2b,\qquad x_1,x_2\in\{-1,0,1\}.
$$

This is a **mathematical measurement model**: it permits a directly observed
signed difference. A ruler may measure an end-to-end sum, but observing $a-b$
with the same noise variance requires a suitable arrangement and sign convention.
Measuring the rods individually and subtracting those readings is not a new,
independent difference measurement under this model.

!!! tip "Core idea"
    Measure $a+b$ and $a-b$. Adding and subtracting those observations recovers
    each length while averaging two independent measurement errors. In matrix
    form, orthogonal design columns give $X^\top X=2I$.

## Short solution

### 1. Establish the baseline

Measuring each rod separately gives

$$
y_1=a+\varepsilon_1,\qquad y_2=b+\varepsilon_2.
$$

The estimators $\hat a=y_1$ and $\hat b=y_2$ are unbiased, with

$$
\operatorname{Var}(\hat a)=\operatorname{Var}(\hat b)=\sigma^2.
$$

This is valid, but each unknown benefits from only one reading. It leaves room
to improve the **design**: the estimators themselves are already natural for
these particular observations.

### 2. Measure the sum and difference instead

Take two fresh measurements:

$$
\begin{aligned}
y_1&=a+b+\varepsilon_1,\\
y_2&=a-b+\varepsilon_2.
\end{aligned}
$$

A compact illustration of the measurement and decoding steps is:

| Measurement | Coefficient of $a$ | Coefficient of $b$ | Observation |
| --- | --- | --- | --- |
| Sum | $+1$ | $+1$ | $y_1=a+b+\varepsilon_1$ |
| Signed difference | $+1$ | $-1$ | $y_2=a-b+\varepsilon_2$ |
| Add readings, then halve | | | $(y_1+y_2)/2$ estimates $a$ |
| Subtract readings, then halve | | | $(y_1-y_2)/2$ estimates $b$ |

Adding the equations cancels $b$; subtracting them cancels $a$:

$$
\hat a=\frac{y_1+y_2}{2}
=a+\frac{\varepsilon_1+\varepsilon_2}{2},
\qquad
\hat b=\frac{y_1-y_2}{2}
=b+\frac{\varepsilon_1-\varepsilon_2}{2}.
$$

### 3. Check unbiasedness explicitly

Linearity of expectation and zero-mean errors give

$$
\begin{aligned}
\mathbb E[\hat a]
&=a+\frac{\mathbb E[\varepsilon_1]+\mathbb E[\varepsilon_2]}{2}=a,\\
\mathbb E[\hat b]
&=b+\frac{\mathbb E[\varepsilon_1]-\mathbb E[\varepsilon_2]}{2}=b.
\end{aligned}
$$

No Gaussian assumption is needed here.

### 4. Calculate both variances

Independence implies $\operatorname{Cov}(\varepsilon_1,\varepsilon_2)=0$. Hence

$$
\begin{aligned}
\operatorname{Var}(\hat a)
&=\frac14\left[\sigma^2+\sigma^2
+2\operatorname{Cov}(\varepsilon_1,\varepsilon_2)\right]
=\frac{\sigma^2}{2},\\
\operatorname{Var}(\hat b)
&=\frac14\left[\sigma^2+\sigma^2
-2\operatorname{Cov}(\varepsilon_1,\varepsilon_2)\right]
=\frac{\sigma^2}{2}.
\end{aligned}
$$

Their covariance is zero as well:

$$
\operatorname{Cov}(\hat a,\hat b)
=\frac14\operatorname{Cov}(\varepsilon_1+\varepsilon_2,
\varepsilon_1-\varepsilon_2)
=\frac14(\sigma^2-\sigma^2)=0.
$$

Zero covariance does not, on its own, establish independence of the estimates.

!!! success "Answer"
    $$
    \boxed{\hat a=\frac{y_1+y_2}{2},\qquad
    \hat b=\frac{y_1-y_2}{2}}
    $$

    $$
    \boxed{\operatorname{Var}(\hat a)=\operatorname{Var}(\hat b)
    =\frac{\sigma^2}{2}}
    $$

    Both variances are half the separate-measurement baseline.

!!! warning "What does doubling accuracy mean?"
    If precision means inverse variance, precision doubles. Standard deviation
    changes from $\sigma$ to $\sigma/\sqrt2$, not $\sigma/2$. Unbiasedness also
    means mean squared error equals variance, so MSE is halved.

## OLS: derive the estimator and covariance

Write the fixed-design linear model as

$$
\mathbf y=X\boldsymbol\theta+\boldsymbol\varepsilon,\qquad
\boldsymbol\theta=\begin{pmatrix}a\\b\end{pmatrix},\qquad
X=\begin{pmatrix}1&1\\1&-1\end{pmatrix}.
$$

Ordinary least squares minimizes the squared residual length:

$$
\hat{\boldsymbol\theta}
=\arg\min_{\boldsymbol\theta}
\|\mathbf y-X\boldsymbol\theta\|_2^2.
$$

Expand the objective and differentiate:

$$
\begin{aligned}
L(\boldsymbol\theta)
&=\mathbf y^\top\mathbf y
-2\boldsymbol\theta^\top X^\top\mathbf y
+\boldsymbol\theta^\top X^\top X\boldsymbol\theta,\\
\nabla L(\boldsymbol\theta)
&=-2X^\top\mathbf y+2X^\top X\boldsymbol\theta.
\end{aligned}
$$

Setting the gradient to zero gives the **normal equations**:

$$
X^\top X\hat{\boldsymbol\theta}=X^\top\mathbf y.
$$

For full-column-rank $X$, $X^\top X$ is positive definite, so this is the unique
minimum and

$$
\boxed{\hat{\boldsymbol\theta}=(X^\top X)^{-1}X^\top\mathbf y}.
$$

Substitute the model:

$$
\hat{\boldsymbol\theta}
=\boldsymbol\theta+(X^\top X)^{-1}X^\top\boldsymbol\varepsilon.
$$

Taking expectations yields $\mathbb E[\hat{\boldsymbol\theta}]=\boldsymbol\theta$.
For $\operatorname{Cov}(\boldsymbol\varepsilon)=\sigma^2I$, let
$K=(X^\top X)^{-1}X^\top$. The covariance follows from a linear transformation:

$$
\begin{aligned}
\operatorname{Cov}(\hat{\boldsymbol\theta})
&=K(\sigma^2I)K^\top\\
&=\sigma^2(X^\top X)^{-1}X^\top X(X^\top X)^{-1}\\
&=\boxed{\sigma^2(X^\top X)^{-1}}.
\end{aligned}
$$

For the sum-and-difference design,

$$
X^\top X=
\begin{pmatrix}1&1\\1&-1\end{pmatrix}
\begin{pmatrix}1&1\\1&-1\end{pmatrix}
=\begin{pmatrix}2&0\\0&2\end{pmatrix}=2I.
$$

Therefore

$$
\hat{\boldsymbol\theta}=\frac12X^\top\mathbf y,
\qquad
\operatorname{Cov}(\hat{\boldsymbol\theta})=\frac{\sigma^2}{2}I.
$$

The two design columns, $(1,1)^\top$ and $(1,-1)^\top$, have zero inner
product. They encode the two parameters orthogonally, so estimating one does not
inflate the variance of the other.

There are two observations and two unknowns: the fitted residual is zero for
any invertible design, but the estimates still contain measurement noise. Zero
residual does **not** mean zero estimation uncertainty, and these two readings
provide no residual degrees of freedom for estimating an unknown $\sigma^2$.

## Compare three measurement designs

The variance entries below are in units of $\sigma^2$.

| Measurements | Design matrix $X$ | Estimators | $\operatorname{Var}(\hat a)/\sigma^2$ | $\operatorname{Var}(\hat b)/\sigma^2$ |
| --- | --- | --- | --- | --- |
| $a$, $b$ | $\begin{pmatrix}1&0\\0&1\end{pmatrix}$ | $y_1$, $y_2$ | $1$ | $1$ |
| $a+b$, $a$ | $\begin{pmatrix}1&1\\1&0\end{pmatrix}$ | $y_2$, $y_1-y_2$ | $1$ | $2$ |
| $a+b$, $a-b$ | $\begin{pmatrix}1&1\\1&-1\end{pmatrix}$ | $(y_1+y_2)/2$, $(y_1-y_2)/2$ | $1/2$ | $1/2$ |

The middle design does not help both rods. Its estimate of $b$ subtracts two
noisy readings without the factor of two, giving variance $2\sigma^2$.
Nonorthogonality entangles the unknowns and can inflate uncertainty.

## Deeper derivations

??? note "Optimality among admissible two-measurement designs"

    Consider any full-rank design

    $$
    X=\begin{pmatrix}x_{11}&x_{12}\\x_{21}&x_{22}\end{pmatrix},
    \qquad x_{ij}\in\{-1,0,1\}.
    $$

    Set

    $$
    A=x_{11}^2+x_{21}^2,\quad
    B=x_{12}^2+x_{22}^2,\quad
    C=x_{11}x_{12}+x_{21}x_{22}.
    $$

    Here $A$ and $B$ are squared column lengths, and $C$ is their inner product.
    Full rank gives $A,B>0$ and $AB-C^2>0$. The coefficient restriction also
    gives $A\le2$ and $B\le2$.

    Inverting the Gram matrix explicitly,

    $$
    X^\top X=\begin{pmatrix}A&C\\C&B\end{pmatrix},\qquad
    (X^\top X)^{-1}
    =\frac1{AB-C^2}\begin{pmatrix}B&-C\\-C&A\end{pmatrix}.
    $$

    Consequently,

    $$
    \operatorname{Cov}(\hat{\boldsymbol\theta})
    =\frac{\sigma^2}{AB-C^2}
    \begin{pmatrix}B&-C\\-C&A\end{pmatrix}.
    $$

    For the first parameter,

    $$
    \operatorname{Var}(\hat a)
    =\frac{\sigma^2B}{AB-C^2}
    \ge\frac{\sigma^2B}{AB}
    =\frac{\sigma^2}{A}
    \ge\frac{\sigma^2}{2}.
    $$

    The first inequality uses $C^2\ge0$: subtracting it makes the positive
    denominator no larger, so the fraction becomes no smaller. The second uses
    $0<A\le2$: taking a reciprocal reverses that size comparison.

    Similarly,

    $$
    \operatorname{Var}(\hat b)
    =\frac{\sigma^2A}{AB-C^2}
    \ge\frac{\sigma^2}{B}
    \ge\frac{\sigma^2}{2}.
    $$

    With $\sigma^2>0$, the first chain attains equality exactly when $C=0$
    and $A=2$; the second requires $C=0$ and $B=2$. Both lower bounds are
    attained simultaneously if and only if

    $$
    \boxed{A=B=2,\quad C=0}
    \qquad\Longleftrightarrow\qquad
    \boxed{X^\top X=2I}.
    $$

    Thus the proposed design is optimal for both marginal variances in the
    class of linear unbiased estimators. It is not unique: row permutations,
    row sign changes, or column sign changes preserve the Gram condition.
    There are multiple equivalent ways to order or sign the measurements.

    This is a componentwise variance result, not a claim that every competing
    covariance matrix minus $(\sigma^2/2)I$ is positive semidefinite.

??? note "The inverse-diagonal inequality and the Schur complement"

    For a symmetric positive definite matrix $M$,

    $$
    \boxed{(M^{-1})_{ii}\ge\frac1{M_{ii}}}.
    $$

    **First, the two-by-two case.** Use $\alpha,\beta,\gamma$ here to avoid
    confusing matrix entries with rod lengths:

    $$
    M=\begin{pmatrix}\alpha&\gamma\\\gamma&\beta\end{pmatrix},
    \qquad
    (M^{-1})_{11}
    =\frac{\beta}{\alpha\beta-\gamma^2}
    =\frac1{\alpha-\gamma^2/\beta}
    \ge\frac1\alpha.
    $$

    Positive definiteness ensures $\beta>0$ and
    $0<\alpha-\gamma^2/\beta\le\alpha$. Equality holds exactly when $\gamma=0$.

    **Now any dimension.** Permute row and column $i$ to the first position and
    partition

    $$
    M=\begin{pmatrix}\alpha&\mathbf r^\top\\\mathbf r&D\end{pmatrix}.
    $$

    The principal submatrix $D$ and its inverse are positive definite. To find
    the first column of $M^{-1}$, solve

    $$
    M\begin{pmatrix}u\\\mathbf v\end{pmatrix}
    =\begin{pmatrix}1\\\mathbf0\end{pmatrix}.
    $$

    The lower block equation gives $\mathbf v=-D^{-1}\mathbf r\,u$.
    Substituting into the upper equation yields

    $$
    (\alpha-\mathbf r^\top D^{-1}\mathbf r)u=1.
    $$

    The denominator is the Schur complement. It is positive: evaluating the
    positive quadratic form of $M$ at $(1,-D^{-1}\mathbf r)^\top$ gives
    $\alpha-\mathbf r^\top D^{-1}\mathbf r>0$. Hence

    $$
    (M^{-1})_{ii}
    =\frac1{\alpha-\mathbf r^\top D^{-1}\mathbf r}
    \ge\frac1\alpha=\frac1{M_{ii}}.
    $$

    Since $D^{-1}$ is positive definite,
    $\mathbf r^\top D^{-1}\mathbf r=0$ if and only if $\mathbf r=\mathbf0$.
    This proves the equality condition: the selected coordinate must have
    zero off-diagonal coupling to all the others.

    Apply the inequality to $M=X^\top X$. If $\mathbf x_i$ is design column $i$,

    $$
    \operatorname{Var}(\hat\beta_i)
    =\sigma^2[(X^\top X)^{-1}]_{ii}
    \ge\frac{\sigma^2}{\|\mathbf x_i\|_2^2}.
    $$

    Orthogonality removes the extra variance caused by coupling to other
    columns. See [Nick Higham's Schur complement explanation](https://nhigham.com/2023/06/01/what-is-the-schur-complement-of-a-matrix/)
    for the underlying block-inverse identity.

??? note "Geometry, multicollinearity, and variance inflation"

    Let $\phi$ be the angle between the columns of $X$. Their inner product is

    $$
    C=\sqrt{AB}\cos\phi.
    $$

    Therefore

    $$
    AB-C^2=AB(1-\cos^2\phi)=AB\sin^2\phi,
    $$

    and the marginal variances become

    $$
    \boxed{\operatorname{Var}(\hat a)=\frac{\sigma^2}{A\sin^2\phi}},
    \qquad
    \boxed{\operatorname{Var}(\hat b)=\frac{\sigma^2}{B\sin^2\phi}}.
    $$

    For fixed column lengths, $\sin^2\phi$ is largest at orthogonality.
    Nearly parallel or antiparallel columns have small $\sin^2\phi$ and large
    estimation variances. Perfect collinearity makes $X$ rank-deficient:
    the two parameters cannot be identified separately.

    The corresponding general variance inflation factor is

    $$
    \mathrm{VIF}_i=\frac1{1-R_i^2}.
    $$

    Here the correct convention is an **uncentered auxiliary regression**:
    regress $\mathbf x_i$ on the other design columns without adding an
    intercept, and define

    $$
    R_i^2
    =1-\frac{\|\mathbf x_i-P_{-i}\mathbf x_i\|_2^2}
                 {\|\mathbf x_i\|_2^2},
    $$

    where $P_{-i}$ projects onto the span of the other columns. Then

    $$
    \operatorname{Var}(\hat\beta_i)
    =\frac{\sigma^2}{\|\mathbf x_i\|_2^2}\,
      \frac1{1-R_i^2}.
    $$

    With only one other column, $R_i^2=\cos^2\phi$. Do not substitute the usual
    centered, intercept-based $R^2$ without changing the model: the sum-and-difference
    example even contains a constant column, so centered conventions can be misleading.

??? note "Generalization: n rods and Hadamard designs"

    Suppose $n$ measurements of $n$ unknown lengths are admissible:

    $$
    \mathbf y=H\boldsymbol\theta+\boldsymbol\varepsilon,
    \qquad H_{ij}\in\{-1,+1\},\qquad H^\top H=nI,
    \qquad \operatorname{Cov}(\boldsymbol\varepsilon)=\sigma^2I.
    $$

    Such an orthogonal sign matrix is a **Hadamard matrix**. The OLS formula gives

    $$
    \hat{\boldsymbol\theta}
    =(nI)^{-1}H^\top\mathbf y
    =\boxed{\frac1nH^\top\mathbf y}.
    $$

    Its covariance is

    $$
    \boxed{\operatorname{Cov}(\hat{\boldsymbol\theta})
    =\frac{\sigma^2}{n}I}.
    $$

    For example, an order-four design is

    $$
    H_4=\begin{pmatrix}
    1&1&1&1\\
    1&-1&1&-1\\
    1&1&-1&-1\\
    1&-1&-1&1
    \end{pmatrix},\qquad H_4^\top H_4=4I.
    $$

    Hadamard designs do not exist for every $n$: for $n>2$, divisibility by four
    is necessary. The displayed construction also extends recursively to powers
    of two via

    $$
    H_{2n}=\begin{pmatrix}H_n&H_n\\H_n&-H_n\end{pmatrix}.
    $$

    These facts do not establish existence for every multiple of four.
    As with two rods, an arbitrary signed combination is a mathematical
    measurement capability, not something automatically available with a ruler.

## Statistical assumptions and limitations

- **Correct fixed-design model and zero-mean errors:** these establish OLS
  unbiasedness. For a random design, the corresponding condition is
  $\mathbb E[\boldsymbol\varepsilon\mid X]=\mathbf0$.
- **Equal variances and uncorrelated errors:**
  $\operatorname{Cov}(\boldsymbol\varepsilon)=\sigma^2I$ gives the covariance
  formula used here. Independence is sufficient, but stronger than needed.
- **No Gaussian requirement for the calculations above:** linearity of expectation
  and covariance algebra suffice.
- **Scope of optimality:** the design lower bounds apply to OLS and, under the
  Gauss–Markov assumptions, linear unbiased estimators. For a square invertible
  two-measurement design, a linear unbiased estimator must satisfy $LX=I$,
  so $L=X^{-1}$ is already unique. The broader BLUE statement is discussed in
  [this Gauss–Markov reference](https://www.statlect.com/fundamentals-of-statistics/Gauss-Markov-theorem).
- **Nonlinear estimators:** a universal lower bound over all nonlinear unbiased
  estimators needs additional distributional assumptions and a separate proof.
  For example, under independent Gaussian errors with known $\sigma^2$, the
  Fisher information is $X^\top X/\sigma^2$ and the regular unbiased-estimator
  Cramér–Rao bound reproduces the OLS covariance bound.
- **Coefficient bounds are essential:** if arbitrary scale $k$ were allowed,
  $X=kI$ would yield covariance $(\sigma^2/k^2)I$ under the same constant-noise
  model. Without a measurement-size or resource constraint, the original
  optimization would have no positive lower bound.
- **Physical noise may differ:** measuring a sum or signed difference can change
  the error variance in real equipment. That would require reanalyzing the design.

??? note "What if the two errors are correlated?"

    If the variances are still $\sigma^2$ but their correlation is $\rho$, then

    $$
    \operatorname{Var}(\hat a)=\frac{\sigma^2}{2}(1+\rho),
    \qquad
    \operatorname{Var}(\hat b)=\frac{\sigma^2}{2}(1-\rho).
    $$

    Positive correlation helps the difference estimate and hurts the sum
    estimate. Both attain $\sigma^2/2$ simultaneously only when $\rho=0$.
    This is why checking the noise covariance is part of the design problem.

## Interview takeaways

| Formula | Meaning |
| --- | --- |
| $\hat{\boldsymbol\theta}=(X^\top X)^{-1}X^\top\mathbf y$ | Decode measurements with OLS. |
| $\operatorname{Cov}(\hat{\boldsymbol\theta})=\sigma^2(X^\top X)^{-1}$ | The Gram matrix controls estimation uncertainty. |
| $(M^{-1})_{ii}\ge1/M_{ii}$ | Off-diagonal coupling cannot improve a coordinate's variance at fixed diagonal strength. |
| $\operatorname{Var}(\hat a),\operatorname{Var}(\hat b)\ge\sigma^2/2$ | Each marginal variance has this lower bound under the coefficient restriction. |
| $X^\top X=2I$ | Both lower bounds are achieved simultaneously. |

> Measure the sum and signed difference instead of one rod per reading.
> Adding and subtracting cancels the unwanted length, so each estimate uses both
> independent noisy observations. The variances halve because the design columns
> are orthogonal and each has squared length two. This relies on the permitted
> coefficients and the same noise variance for every measurement.

[Related note: least squares from first principles](../../notes/mathematics.md)

---

[Day 3 overview](index.md) · [Next question](question-2.md)
