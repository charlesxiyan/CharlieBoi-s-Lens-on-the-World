# Day 4 · Spherical Coordinate Variance

*Edited by Charles*

**Statistics · Symmetry · Expectation · Variance**

[Original question on OpenQuant](https://openquant.co/questions/spherical-coordinate-variance)

## Problem

Choose a point $(X,Y,Z)$ uniformly by surface area on the **surface of the unit
sphere**, centered at the origin. Find $\operatorname{Var}(Z)$.

The source says a random point "on the unit sphere"; here we make the intended
uniform-surface assumption explicit. Choosing uniformly inside the solid ball
or choosing the polar angle uniformly is a different experiment.

!!! tip "Core idea"
    Every sampled point satisfies $X^2+Y^2+Z^2=1$. Uniform surface sampling treats
    all three axes identically, so their expected squared coordinates are equal.
    They must therefore share the total expectation of one equally.

## Short solution

### 1. Start from the definition of variance

$$\operatorname{Var}(Z)=E[Z^2]-(E[Z])^2.$$

We need the mean and the second moment separately. A mean of zero does not
imply a second moment of zero.

### 2. Show that the mean is zero

Reflecting any surface patch through the plane $z=0$ preserves its area and
changes $Z$ to $-Z$. Uniform sampling therefore gives $Z$ and $-Z$ the same
 distribution. Consequently,

$$E[Z]=E[-Z]=-E[Z]\quad\Longrightarrow\quad E[Z]=0.$$

### 3. Explain exactly why E[Z²] = 1/3

**First, use the sphere equation.** For every outcome, not merely on average,

$$X^2+Y^2+Z^2=1.$$

Take expectations of both sides and use linearity:

$$E[X^2]+E[Y^2]+E[Z^2]=E[1]=1.$$

Linearity of expectation does **not** require independence. In fact, the
coordinates are constrained by the sphere equation and are not independent.

**Next, use rotational symmetry.** Swapping coordinate axes preserves both the
sphere and its uniform surface-area distribution. No axis is favored, so
$X$, $Y$, and $Z$ have the same marginal distribution. Thus

$$E[X^2]=E[Y^2]=E[Z^2].$$

Write their common value as $a$. The previous equation becomes

$$a+a+a=1\quad\Longrightarrow\quad a=\frac13.$$

Therefore,

$$\boxed{E[Z^2]=\frac13}.$$

This says the **average squared height over random surface points** is $1/3$.
It does not say that every point has $Z^2=1/3$. At a pole $Z^2=1$; at the equator
$Z^2=0$. Those different outcomes average to $1/3$ under uniform surface sampling.

### 4. Substitute into the variance formula

!!! success "Answer"
    $$\boxed{\operatorname{Var}(Z)=\frac13-0^2=\frac13}$$

!!! warning "Watch out"
    $E[Z^2]$ and $(E[Z])^2$ are different quantities: here they are $1/3$ and $0$.
    The sphere equation alone is also insufficient: a distribution concentrated
    at the poles still lies on the sphere but has $E[Z^2]=1$. Uniform rotational
    symmetry is what makes the three second moments equal.

??? note "Independent derivation: Z is uniform on [-1, 1]"
    Use polar angle $\theta\in[0,\pi]$ and azimuth $\phi\in[0,2\pi)$.
    On the unit sphere, $z=\cos\theta$ and the area element is

    $$dA=\sin\theta\,d\theta\,d\phi.$$

    Since $|dz|=\sin\theta\,d\theta$, integrating around a full horizontal
    band gives area $2\pi\,dz$ for a height interval of width $dz$.
    Divide by the total area $4\pi$:

    $$f_Z(z)=\frac{2\pi}{4\pi}=\frac12,\qquad -1\le z\le1.$$

    Thus $Z\sim\operatorname{Uniform}[-1,1]$, and directly

    $$E[Z^2]=\int_{-1}^{1}z^2\frac12\,dz
    =\frac12\left[\frac{z^3}{3}\right]_{-1}^{1}=\frac13.$$

    Although a band near a pole has a smaller circumference, its surface width
    per unit change in height is larger. The factors cancel, giving equal
    surface area for equal height intervals.

??? note "Derivation by conditioning: integrate over the polar angle"
    **1. Find the joint law of the angles.** Write a surface point as
    $(X,Y,Z)=(\sin\Theta\cos\Phi,\ \sin\Theta\sin\Phi,\ \cos\Theta)$.
    Uniform surface sampling means probability $=$ area $/\,4\pi$, and
    $dA=\sin\theta\,d\theta\,d\phi$, so

    $$
    f_{\Theta,\Phi}(\theta,\phi)=\frac{\sin\theta}{4\pi}
    =\underbrace{\frac{\sin\theta}{2}}_{f_\Theta(\theta)}
    \cdot\underbrace{\frac{1}{2\pi}}_{f_\Phi(\phi)},
    \qquad 0\le\theta\le\pi,\ 0\le\phi<2\pi.
    $$

    The density factorizes, so $\Theta$ and $\Phi$ are independent and
    $\Phi\mid\Theta=\theta\sim\operatorname{Uniform}[0,2\pi)$.
    As a check, the cap $\{\Theta\le t\}$ has probability
    $\int_0^t\tfrac12\sin\theta\,d\theta=\tfrac{1-\cos t}{2}$, which goes from $0$ to $1$.

    **2. Condition on the polar angle.** Given $\Theta=\theta$, the height is fixed:
    $Z=\cos\theta$. Hence

    $$
    E[Z^2\mid\Theta=\theta]=\cos^2\theta.
    $$

    **3. Average over $\Theta$ (law of total expectation).**

    $$
    E[Z^2]=E\big[E[Z^2\mid\Theta]\big]
    =\int_0^\pi\cos^2\theta\cdot\frac{\sin\theta}{2}\,d\theta.
    $$

    Substitute $u=\cos\theta$, $du=-\sin\theta\,d\theta$:

    $$
    E[Z^2]=\frac12\int_{-1}^{1}u^2\,du=\frac12\cdot\frac23=\frac13.
    $$

    **4. Consistency check with $X$.** Conditioning matters more here, because $X$
    still depends on $\Phi$ once $\Theta$ is fixed:

    $$
    E[X^2\mid\Theta=\theta]=\sin^2\theta\;E[\cos^2\Phi]=\frac{\sin^2\theta}{2},
    $$

    $$
    E[X^2]=\int_0^\pi\frac{\sin^2\theta}{2}\cdot\frac{\sin\theta}{2}\,d\theta
    =\frac14\int_0^\pi\sin^3\theta\,d\theta=\frac14\cdot\frac43=\frac13.
    $$

    This matches $E[Z^2]$, as the symmetry argument predicts, and
    $E[X^2]+E[Y^2]+E[Z^2]=1$.

??? note "Why a uniform polar angle gives the wrong experiment"
    If instead $\theta\sim\operatorname{Uniform}[0,\pi]$, then

    $$E[\cos^2\theta]=\frac1\pi\int_0^\pi\cos^2\theta\,d\theta=\frac12.$$

    That sampling scheme overweights the poles. For uniform surface sampling,
    the polar angle density is $f_\Theta(\theta)=\tfrac12\sin\theta$,
    not a constant. Uniform $Z$ and uniform azimuth produce the intended
    surface distribution.

??? note "Generalization: a sphere of radius R"
    For a uniformly sampled surface point on a sphere of radius $R$ centered
    at the origin, symmetry gives $E[Z]=0$ and
    $3E[Z^2]=R^2$. Hence $\operatorname{Var}(Z)=R^2/3$.

---

[Previous question](question-3.md) · [Day 4 overview](index.md) · [Next question](question-5.md)
