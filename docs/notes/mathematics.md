# Mathematics: least squares

Suppose we want a line $\hat{y}_i = ax_i + b$ that approximates observations
$(x_i, y_i)$. Least squares minimizes the sum of squared residuals:

$$
L(a,b) = \sum_{i=1}^{n} \left(y_i - (ax_i + b)\right)^2.
$$

## From derivatives to a solution

Setting the partial derivatives to zero gives

$$
\begin{aligned}
a\sum_i x_i^2 + b\sum_i x_i &= \sum_i x_i y_i, \\
a\sum_i x_i + nb &= \sum_i y_i.
\end{aligned}
$$

When the $x_i$ are not all identical, the solution is

$$
a = \frac{\sum_i (x_i-\bar{x})(y_i-\bar{y})}
         {\sum_i (x_i-\bar{x})^2},
\qquad b = \bar{y} - a\bar{x}.
$$

!!! question "What if all inputs are the same?"
    The denominator becomes zero. The data cannot determine a unique slope.
    This is a useful reminder to check assumptions before applying a formula.
