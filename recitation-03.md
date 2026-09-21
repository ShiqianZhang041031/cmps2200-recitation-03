# CMPS 2200 Recitation 03

**Name:** Shiqian Zhang

In this recitation, we will investigate recurrences for work and span of algorithms. Unlike other recitations, you may add your answers directly to this document. You do not need to use an `answers.md`.

## Tree method (9 pts)

Solve the following recurrences using the tree method.

### a) W(n) = 3W(n/4) + n^2

At the root, the nonrecursive work is

```math
n^2.
```

At level 1, there are 3 subproblems of size $n/4$, so the total work at that level is

```math
3\left(\frac{n}{4}\right)^2
=
n^2\frac{3}{16}.
```

At level 2, there are $3^2$ subproblems of size $n/4^2$, giving

```math
3^2\left(\frac{n}{4^2}\right)^2
=
n^2\left(\frac{3}{16}\right)^2.
```

In general, at level $i$ there are $3^i$ subproblems, each of size $n/4^i$. Therefore, the total work at level $i$ is

```math
3^i\left(\frac{n}{4^i}\right)^2
=
n^2\left(\frac{3}{16}\right)^i.
```

The recursion tree has height

```math
\log_4 n.
```

Therefore, the total nonrecursive work is

```math
n^2
\left(
1
+
\frac{3}{16}
+
\left(\frac{3}{16}\right)^2
+
\cdots
\right).
```

Since $3/16 < 1$, this is a decreasing geometric series whose sum is $\Theta(n^2)$.

The leaves contribute

```math
3^{\log_4 n}
=
n^{\log_4 3},
```

which is asymptotically smaller than $n^2$.

Therefore,

```math
W(n) = \Theta(n^2).
```

---

### b) W(n) = W(n/3) + W(2n/3) + n log n

At the root, the work is

```math
n\log n.
```

The two children have sizes $n/3$ and $2n/3$. Notice that their sizes add to

```math
\frac{n}{3}+\frac{2n}{3}=n.
```

This remains true at every level: the sum of the sizes of all subproblems at a given level is $n$.

For a subproblem of size $m$, the nonrecursive work is

```math
m\log m.
```

Therefore, the work at a level is the sum of $m\log m$ over all subproblems at that level.

Since every subproblem has size at most $n$,

```math
m\log m \leq m\log n.
```

Because the subproblem sizes at each level sum to $n$, the total work at any level is at most

```math
n\log n.
```

The longest recursive path repeatedly takes the $2n/3$ child, so the height of the tree is

```math
\Theta(\log n).
```

Thus, an upper bound on the total work is

```math
O(n\log^2 n).
```

For a lower bound, consider the first

```math
\frac{1}{2}\log_3 n
```

levels.

At these levels, even the smallest possible subproblem has size at least

```math
\frac{n}{3^{(1/2)\log_3 n}}
=
\sqrt{n}.
```

Therefore,

```math
\log m \geq \frac{1}{2}\log n.
```

Since the subproblem sizes still sum to $n$, every one of these levels has total work at least

```math
\frac{1}{2}n\log n.
```

There are $\Theta(\log n)$ such levels, so

```math
W(n)=\Omega(n\log^2 n).
```

Combining the upper and lower bounds gives

```math
W(n)=\Theta(n\log^2 n).
```

---

### c) W(n) = 2W(n/2) + n/log n

For convenience, assume $n$ is a power of 2.

At the root, the nonrecursive work is

```math
\frac{n}{\log n}.
```

At level 1, there are 2 subproblems of size $n/2$. Their total nonrecursive work is

```math
2
\left(
\frac{n/2}{\log(n/2)}
\right)
=
\frac{n}{\log(n/2)}.
```

At level $i$, there are $2^i$ subproblems, each of size $n/2^i$. Therefore, the total work at level $i$ is

```math
2^i
\left(
\frac{n/2^i}{\log(n/2^i)}
\right)
=
\frac{n}{\log(n/2^i)}.
```

Using base-2 logarithms and letting

```math
L=\log_2 n,
```

the level-$i$ cost becomes

```math
\frac{n}{L-i}.
```

The recursion has $\Theta(\log n)$ levels. Summing the level costs gives

```math
n
\left(
\frac{1}{L}
+
\frac{1}{L-1}
+
\cdots
+
\frac{1}{1}
\right).
```

This is

```math
nH_L,
```

where $H_L$ is the $L$-th harmonic number.

Since

```math
H_L=\Theta(\log L),
```

and $L=\log n$, we get

```math
H_L=\Theta(\log\log n).
```

Therefore,

```math
W(n)=\Theta(n\log\log n).
```

The leaves contribute only $\Theta(n)$, so they do not change the final bound.

---

## Brick method (6 pts)

Solve the following recurrences using the brick method. First argue whether they are root-dominated, leaf-dominated, or balanced. Then state the resulting asymptotic bound for $W(n)$.

### d) W(n) = 2W(0.49n) + 1.01n

The root contributes linear work:

```math
1.01n.
```

At the next level, there are two subproblems of size $0.49n$.

The total size of the children is

```math
2(0.49n)=0.98n.
```

Therefore, the total linear work shrinks by a factor of $0.98$ from one level to the next.

The level costs have the form

```math
1.01n,
```

```math
1.01n(0.98),
```

```math
1.01n(0.98)^2,
```

and so on.

Because

```math
0.98 < 1,
```

the work decreases geometrically down the tree.

Thus, this recurrence is **root-dominated**.

The total work is a geometric series:

```math
1.01n
\left(
1+0.98+0.98^2+\cdots
\right).
```

Therefore,

```math
W(n)=\Theta(n).
```

---

### e) W(n) = W(n/2) + W(n/4) + 0.999n

The root contributes

```math
0.999n
```

work.

The two recursive children have sizes

```math
\frac{n}{2}
```

and

```math
\frac{n}{4}.
```

Their total size is

```math
\frac{n}{2}+\frac{n}{4}
=
\frac{3n}{4}.
```

Thus, the total linear work at the next level is only $3/4$ of the previous level's work.

The level costs decrease like

```math
0.999n,
```

```math
0.999n\left(\frac{3}{4}\right),
```

```math
0.999n\left(\frac{3}{4}\right)^2,
```

and so on.

Since

```math
\frac{3}{4}<1,
```

the work decreases geometrically.

Therefore, this recurrence is **root-dominated**.

The total work is

```math
0.999n
\left(
1
+
\frac{3}{4}
+
\left(\frac{3}{4}\right)^2
+
\cdots
\right).
```

Hence,

```math
W(n)=\Theta(n).
```

---

## Bonus (3 pts)

### f) W(n) = sqrt(n) W(sqrt(n)) + sqrt(n)

We have

```math
W(n)
=
\sqrt{n}W(\sqrt{n})
+
\sqrt{n}.
```

Expand the recurrence once:

```math
W(\sqrt{n})
=
n^{1/4}W(n^{1/4})
+
n^{1/4}.
```

Substituting this into the original recurrence gives

```math
W(n)
=
n^{3/4}W(n^{1/4})
+
n^{3/4}
+
n^{1/2}.
```

Expanding again gives

```math
W(n)
=
n^{7/8}W(n^{1/8})
+
n^{7/8}
+
n^{3/4}
+
n^{1/2}.
```

After $k$ expansions, the coefficient of the remaining recursive term is

```math
n^{1-1/2^k}.
```

The recursion stops when

```math
n^{1/2^k}
```

becomes a constant.

Taking logarithms,

```math
\frac{\log n}{2^k}=\Theta(1),
```

so

```math
2^k=\Theta(\log n),
```

and therefore

```math
k=\Theta(\log\log n).
```

At the bottom of the tree,

```math
n^{1-1/2^k}=\Theta(n).
```

The additive terms are

```math
n^{1/2},
\quad
n^{3/4},
\quad
n^{7/8},
\quad
\ldots
```

Near the bottom, these can be written as

```math
\frac{n}{2},
\quad
\frac{n}{4},
\quad
\frac{n}{16},
\quad
\ldots
```

up to constant factors.

Their sum is therefore $\Theta(n)$.

Thus,

```math
W(n)=\Theta(n).
```