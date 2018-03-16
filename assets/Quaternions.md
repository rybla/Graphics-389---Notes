# Quaternions

Can be used to represent rotations in 3-space.

### Properties:

- Quaternion has form `ai + bj + ck + d = Q(a b c d)`
- `i^2 = j^2 = k^2 = -1`
- Composing quaternions corresponds to composing rotations:

    |forward | backward|
    |--------|---------|
    |`ij = k`|`ji = -k`|
    |`jk = i`|`kj = -i`|
    |`ki = j`|`ik = -j`|

- Important properties:
```
Q(a b c d) * Q(e f g h) = Q((a,b,c,d) • (c,d,e,f)) // dot product

Vec (a,b,c) = Q(0 a b c) // vector -> quaternion

vecPart Q(a b c d) = (b,c,d)

rePart $ q * p = Q(a b c d) * Q(e f g h) = ae - bf - cg - dh = ae - Vec(q)•Vec(p)

iPart $ q * p = af + eb + (ch - gd) // ? + determinant
jPart $ q * p = ...
kPart $ q * p = ...

q * p = (s1s2 - u1•u2, s2u1 + s1u2 + u1 x u2)
    where
        q = (s1,u1) // (rePart q, vecPart q)
        p = (s2,u2) // (rePart p, vecPart p)

// inverse of quaternion
inv q = inv Q(a b c d) = (1/|q|^2) (rePart q, - vecPart q)
```

## To Rotation

To reprsent a rotation in 3-space, take a unit vector v (axis of rotation) and choose angle t. Write that as a quaternion

```
r = (cos(t/2), sin(t/2) v) // unit vector
|r|^2 = cos(t/2)^2 + sin(t/2)^2 |v|^2 = 1
inv r = (cos(t/2), -sin(t/2) v)
```

## Rotate a Vector

```
r = rotation quaternion
w = vector to rotate

q' = r * (Vec w) * inv r // rotation within quaternions

// or more simply

w' = R w // rotation for vector
```