# B-Splines

## Piecewise Bezier Curves

_Recall:_ subdivision process for quadratic bezier.

![](images/subcurve1.png)

The goal is to approximate a control polygon with smoothly varying curve. But a problem with Bezier solution is non-control ⟹ every point (except end points) has influence over entire curve, so it's hard to work with to fine tune local sections.

## Quadratic Beziers

So instead we stitch together a series of Bezier curves (quadratic Bezier pieces). This results in local controls over the curves, and two degrees of continuity:

- `C^0` : line is cont.
- `C^1` : first derivatives
- `G^1` : instead of `C^1`, can have geometric continuity, `seg.(n+1)'(1) = k seg.n'(0)`

![](images/bspline1.png)

## Cubic Beziers

Can also have cubic Bezier spline, where the black dots are the original control polygon, and the blue arrows are tangent vectors that define the rest of the points of the cubic Bezier control polygon:

![](images/bspline2.png)

## Family of Curves

B-splines of order `k = 2,3,...`. 

```
Q(u) = sum([ b.i(u) P.i for i=1..n ]) where
b.i(u) = b(u - i)
```

# B-Spline Basis Functions

The `b(u)` is the B-spline basis functions for a given `k`. It is a piecewise polynomial over the "knot" intervals. Example of `k = 4`, where the red points are knot points:

![](images/bspline3.png)

- degree of polynomial segments is `k - 1`.
- `C^(k-2)` continuity at knot points (where segments meet).

B-spline basis functions for `k = 2`:

![](images/bspline4.png)

B-spline basis functions for `k = 3`:

![](images/bspline5.png)

Each of these basis functions are piecewise of `k` parts, and gets smoother and smoother as you increase `k`.

Applying to control polygon with linear basis functions:

![](images/bspline6.png)

Applying to control polyogn with quadratic bassi functions:

![](images/bspline7.png)

However, setup of `c(t) = sum([ b(t-i) p.i for i=0..(n-1) ])` is illegal since you can't really sum points. So this only works if it's an _affine combination_. And, it turns out that we can have it such, since

```
all([ sum([ p.i(t) for i=1..k ]) == 1 for t in [0,1] ]).
```


So the ultimate process is solvign for the polynomial coefficients of each peice:

![](images/bspline8.png)


You also get this, from the affine property:

```
∫ b(t)dt = 1.
```

You can think of a type of inner product in the space of functions:

```
<f,g> = ∫ f(t)g(t)dt
```

`g(t)` is `sum([ u.i v.i for i=1..n ]).` _Normalizing:_ unit area functions, `∫ f(t)dt = 1`.

### Convolution

Define the convolution of `f` with `g` as

```
(f * g)(s) = ∫ f(t) g(s-t) dt.
```

where `g(s-t)` is the analysis filter of `g`, and `s` is arbitrary. Then what this does is reflects and translates `g` like so:

| ![](images/bspline9.png) | ----> | ![](images/bspline10.png) |
|---|---|---|


*Fact:* `∫ (g*f)(t)dt = ∫ f(t)dt` if `g` unit area. Integral is preserved.

For example, define `k = 1` B-spline basis function

![](images/bspline11.png)

```
b.(i+1)(t) = (b.i * b.0)(t) // basis functions
```