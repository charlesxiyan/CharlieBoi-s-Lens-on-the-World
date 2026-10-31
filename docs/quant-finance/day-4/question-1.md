---
title: Explaining Returns — Regression, Transformations, and Model Comparison
description: A three-part interview question on return regressions, normality and p-values, log returns, and marginal versus conditional associations.
tags:
  - Statistics
  - Linear Regression
  - Statistical Inference
  - Log Returns
  - Model Comparison
---

# Day 4 · Explaining Returns

*Edited by Charles*

**Statistics · Linear Regression**

Source: Charles's interview discussion, developed into a three-part review exercise.

## Problem

You observe three time-aligned series of **simple returns**, $x_{1,t}$,
$x_{2,t}$, and $y_t$. The data are already cleaned and returns are expressed as
decimals. Use $x_1$ and $x_2$ to explain $y$.

### Part 1 — Modeling and inference

1. What model would you start with, and how would you interpret its coefficients?
2. How would you assess explanatory power and coefficient significance?
3. Must the predictors or response be normal? What role does conditional error
   normality play in exact finite-sample inference?
4. How can the CLT support large-sample inference, and under what conditions?
5. Does explaining returns establish causality?

### Part 2 — Transforming every return

A candidate proposes $\tilde r=\log(1+r)$ for **all three** return series and fits

$$
\tilde y_t=a+b_1\tilde x_{1,t}+b_2\tilde x_{2,t}+u_t.
$$

Assume every return is strictly greater than $-1$. The candidate argues that
regression requires normally distributed explanatory variables.

1. Is that reasoning correct, and what does this transformation represent?
2. Does the same transformation preserve the original linear relationship or coefficients?
3. How should the transformed coefficients be interpreted?
4. Does the transformation guarantee normality or valid p-values?
5. When might the original and transformed regressions be similar or different?
6. What modeling considerations could justify log returns?

### Part 3 — Comparing three regressions

Using the same observations and an intercept in each model, consider

$$
\begin{aligned}
A:\quad y_t&=\alpha_A+c_1x_{1,t}+e_{A,t},\\
B:\quad y_t&=\alpha_B+c_2x_{2,t}+e_{B,t},\\
C:\quad y_t&=\alpha_C+\beta_1x_{1,t}+\beta_2x_{2,t}+e_{C,t}.
\end{aligned}
$$

Here $c_1,c_2$ denote univariate slopes, distinct from the log-model slopes above.

1. Are all three necessary? What distinct question does each answer?
2. How do marginal and conditional coefficient interpretations differ?
3. Assuming $y=\alpha+\beta_1x_1+\beta_2x_2+\varepsilon$ and
   $E[\varepsilon\mid x_1,x_2]=0$, derive

    $$c_1=\beta_1+\beta_2\frac{\operatorname{Cov}(x_1,x_2)}{\operatorname{Var}(x_1)}.$$

    When does $c_1=\beta_1$?

4. Why might significance disappear after adding the other predictor?
5. Why is a higher in-sample $R^2$ insufficient to justify an added variable?
6. How does strong correlation between the predictors affect estimation?

!!! tip "Core idea"
    Separate three decisions: what relationship to model, which assumptions
    support inference, and what each model comparison tells you.
    OLS does not require normal predictors. Taking logs changes the model;
    controlling for another predictor changes the meaning of its coefficient.

## Short solution

### 1. Start with the multiple regression

Use

$$y_t=\alpha+\beta_1x_{1,t}+\beta_2x_{2,t}+\varepsilon_t.$$

Under $E[\varepsilon_t\mid x_{1,t},x_{2,t}]=0$, the right-hand linear part is
$E[y_t\mid x_{1,t},x_{2,t}]$. Holding $x_2$ fixed, a **one-percentage-point**
increase in $x_1$ is associated with a $\beta_1$-percentage-point increase in
expected $y$. The intercept is the expected return when both explanatory
returns are zero; this interpretation may involve extrapolation if that point
lies outside the observed support.

These are conditional associations or, for financial factors, return exposures.
Causality requires additional identification assumptions. Even when the
conditional mean is nonlinear, OLS can describe a best linear projection,
but its coefficients then need that more limited interpretation.

Assess $R^2$, adjusted $R^2$, coefficient sizes and confidence intervals, and
residual behavior. For prediction, also use chronological out-of-sample evaluation.

### 2. Identify which distribution matters for p-values

The normality requirement in classical exact tests concerns **conditional errors**,
not the marginal distributions of $x_1$, $x_2$, or $y$.
Under $\varepsilon\mid X\sim N(0,\sigma^2I)$ and full column rank,

$$
T_j=\frac{\hat\beta_j-\beta_{j,0}}
{s\sqrt{[(X^\top X)^{-1}]_{jj}}}
\ \bigg|\ X\sim t_{n-3}
$$

under the null, where $s^2=\sum_t\hat\varepsilon_t^2/(n-3)$ and $n>3$.
The three fitted parameters are the intercept and two slopes.

Normality is not necessary to compute OLS. Without normal errors, suitable
large-sample conditions can instead give an approximately normal test statistic.
The standard error must match the error structure: heteroskedasticity-robust
inference for suitable independent observations, or an appropriate HAC estimator
for weakly dependent time series. A large sample alone does not fix endogeneity,
strong dependence, or failures of the necessary moment conditions.

[Duke's regression notes](https://www2.stat.duke.edu/courses/Fall25/sta521.001/slides/properties-ols.html)
provide the conditional normal model and the transition to asymptotic inference.

??? note "Derivation: where the CLT enters OLS"
    Write $z_t=(1,x_{1,t},x_{2,t})^\top$ and
    $\theta=(\alpha,\beta_1,\beta_2)^\top$. Substituting the model into OLS gives

    $$
    \hat\theta-\theta
    =\left(\sum_tz_tz_t^\top\right)^{-1}\sum_tz_t\varepsilon_t.
    $$

    Multiply by $\sqrt n$ and regroup:

    $$
    \sqrt n(\hat\theta-\theta)
    =\left(\frac1n\sum_tz_tz_t^\top\right)^{-1}
    \left(\frac1{\sqrt n}\sum_tz_t\varepsilon_t\right).
    $$

    Suppose the first average converges in probability to a finite positive
    definite matrix $Q$, and the second term satisfies a CLT with covariance $S$.
    These require appropriate exogeneity, moment, and dependence conditions.
    Slutsky's theorem then gives

    $$\sqrt n(\hat\theta-\theta)\xrightarrow{d}N(0,Q^{-1}SQ^{-1}).$$

    For independent observations, $S=E[z_tz_t^\top\varepsilon_t^2]$ when
    $E[z_t\varepsilon_t]=0$. For suitable dependent time series it is the
    long-run covariance of $z_t\varepsilon_t$.

    With a consistent covariance estimate and nonzero limiting variance,
    $(\hat\beta_j-\beta_{j,0})/\widehat{\operatorname{SE}}(\hat\beta_j)$
    converges to $N(0,1)$ under the null. A two-sided large-sample p-value is
    approximately $2[1-\Phi(|T_j|)]$.

    This is a statement about repeated-sample estimation errors. Collecting
    more observations or standardizing individual returns does not make those
    returns normal. Particularly heavy tails or dominant observations can make
    a normal approximation poor or invalidate its assumptions.

### 3. Interpret the log transformation as a new specification

For a simple return $r_t=P_t/P_{t-1}-1$,

$$\log(1+r_t)=\log P_t-\log P_{t-1}.$$

This is a log return, defined here only for $r_t>-1$. It is not a guaranteed
normalizing transformation. Exponentiating the proposed model gives

$$1+y=e^a(1+x_1)^{b_1}(1+x_2)^{b_2}e^u.$$

The model now describes a multiplicative relationship among gross returns.
Applying the same nonlinear transformation to every variable does not preserve
an additive linear equation, its coefficients, or its p-values.

When the conditional mean in log space is correctly specified,

$$
\frac{\partial E[\log(1+y)\mid X]}{\partial\log(1+x_j)}=b_j.
$$

The fitted gross-return curve $\widehat G_y=e^a(1+x_1)^{b_1}(1+x_2)^{b_2}$
has elasticity $b_j$ with respect to $1+x_j$. This is **not** the elasticity
of the net return $y$ with respect to $x_j$.

For small returns, $\log(1+r)=r-r^2/2+O(r^3)\approx r$, so well-conditioned
regressions may produce similar estimates. Large returns, returns near $-1$,
or an ill-conditioned design can produce much larger differences.

Log returns add across time; simple returns aggregate across assets with
beginning-of-period portfolio weights. Choose the representation for the
financial question and model fit, not a nonexistent normal-predictor requirement.

??? note "Retransformation: why exponentiating is not enough"
    Let $m(X)=a+b_1\log(1+x_1)+b_2\log(1+x_2)$ and $E[u\mid X]=0$.
    Provided the expectation exists,

    $$E[1+y\mid X]=e^{m(X)}E[e^u\mid X].$$

    The factor $E[e^u\mid X]$ is generally not one and can depend on $X$.
    If $u\mid X\sim N(0,\tau^2)$ with constant variance, it equals
    $e^{\tau^2/2}$. Exponentiating the fitted log mean alone therefore does not
    generally estimate the conditional mean simple return.

    For the fitted net-return curve $\hat y=\widehat G_y-1$,

    $$\frac{\partial\hat y}{\partial x_j}
    =b_j\frac{1+\hat y}{1+x_j}.$$

    This shows why $b_j$ is not a constant percentage-point sensitivity in
    the original simple-return units.

### 4. Use the single-predictor models as comparisons

| Model | Main question |
| --- | --- |
| $y\sim x_1$ | What is the marginal linear association with $x_1$? |
| $y\sim x_2$ | What is the marginal linear association with $x_2$? |
| $y\sim x_1+x_2$ | What is each association after controlling for the other? |

All three models can be informative, but only the multiple regression is needed
to estimate the joint linear specification. Their slopes need not match.

For the univariate population slope, assuming finite second moments and
$\operatorname{Var}(x_1)>0$,

$$
\begin{aligned}
c_1
&=\frac{\operatorname{Cov}(x_1,y)}{\operatorname{Var}(x_1)}\\
&=\frac{\beta_1\operatorname{Var}(x_1)
+\beta_2\operatorname{Cov}(x_1,x_2)
+\operatorname{Cov}(x_1,\varepsilon)}{\operatorname{Var}(x_1)}\\
&=\beta_1+\beta_2\frac{\operatorname{Cov}(x_1,x_2)}{\operatorname{Var}(x_1)}.
\end{aligned}
$$

The last step uses $E[\varepsilon\mid x_1,x_2]=0$. Thus $c_1=\beta_1$ if
$\beta_2=0$ or $\operatorname{Cov}(x_1,x_2)=0$. This equality concerns population
slopes; finite-sample estimates may still differ.

For example, if $\beta_1=\beta_2=1$, both predictors have variance $1$, and
their correlation is $0.8$, then $c_1=1.8$ while $\beta_1=1$.
The univariate slope includes their shared movement. This is an omitted-variable
effect relative to estimating $\beta_1$; it does not make the marginal
association an invalid descriptive quantity.

### 5. Evaluate incremental information, not just a higher R²

After adding a predictor, an existing coefficient can change because the new
model separates shared associations. Its standard error can also increase
because correlated regressors provide little independent variation. Either can
make a previously significant coefficient insignificant.

With identical observations and an intercept, ordinary in-sample $R^2$ cannot
decrease when a regressor is added: the larger model can always set its new
coefficient to zero and reproduce the smaller fit. Instead, consider the added
coefficient's size and uncertainty, an appropriate conditional test, adjusted
$R^2$, and out-of-sample performance if prediction is the goal.

Strong collinearity makes separate coefficients imprecise and potentially
unstable, but does not itself bias OLS under exogeneity. Predictors may remain
jointly useful. Perfect collinearity prevents unique estimation of their
individual coefficients.

??? note "Nested-model test and collinearity formula"
    For adding $x_2$ to the model containing $x_1$, the classical partial F-test is

    $$F=\frac{(\mathrm{RSS}_A-\mathrm{RSS}_C)/1}{\mathrm{RSS}_C/(n-3)}.$$

    Under the classical normal, homoskedastic, independent-error assumptions
    and $H_0:\beta_2=0$, this follows $F_{1,n-3}$ and equals the square of the
    corresponding classical coefficient t-statistic. With heteroskedasticity or
    serial dependence, use an appropriate robust Wald test instead of treating
    this RSS formula as an exact F-test.

    For two regressors under homoskedastic uncorrelated conditional errors,

    $$
    \operatorname{Var}(\hat\beta_1\mid X)
    =\frac{\sigma^2}{\sum_t(x_{1,t}-\bar x_1)^2(1-r_{12}^2)},
    $$

    where $r_{12}$ is their sample correlation. As its magnitude approaches one,
    precision deteriorates for a given amount of variation in $x_1$.

!!! success "Interview answer"
    I would start with multiple regression to estimate conditional return
    associations. Normal regressors are not required. Inference needs suitable
    assumptions and standard errors; large samples can support asymptotic tests.
    Log returns are a modeling choice. The two univariate regressions help
    distinguish marginal associations from each predictor's additional information.

!!! warning "Mistakes to avoid"
    Do not take logs merely to satisfy a supposed normality requirement on $X$.
    Do not say that the CLT makes raw observations normal, or that more data
    guarantees reliable p-values. Comparing three regressions is useful when
    their different interpretations are explained; listing models alone is incomplete.

??? note "Personal review: the original reasoning and its correction"
    In the interview, I considered three regressions and proposed converting
    returns with $\log(1+r)$. The mistake was associating regression's normality
    discussion with the predictors. The transformation can still be meaningful,
    but needs a modeling reason. The three regressions can also be useful,
    provided I explain marginal versus conditional associations.

    The initial question was open-ended: how to use two returns to interpret a
    third. The normality and p-value discussion arose later. The expanded
    questions above are a review exercise, not a verbatim interview transcript.

---

[Day 4 overview](index.md) · [Back to the study plan](../study-plan/index.md)
