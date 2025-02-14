# QR factorization

The [QR factorization](https://en.wikipedia.org/wiki/QR_decomposition) is an algorithm for breaking down a matrix `A` into two sub-matrixes `Q` and `R` such that:
- `A = QR`
- `Q` is orthonormal (`Q.T Q = Q Q.T = I`)
- `R` is upper-triangular (all elements below the diagonal are 0)

Such a decomposition is the starting point for one of the [algorithms](https://en.wikipedia.org/wiki/QR_algorithm) used to compute eigenvalues and eigenvectors of arbitrarily large matrixes, and for one of the algorithms to solve the [linear least squares](https://en.wikipedia.org/wiki/Linear_least_squares) problem.

The decomposition uses some auxiliary operations:
- the *L_2* Euclidian norm of a vector `x`
- the vector normalization `x`
- the projection of a vector `x` onto a vector `y`

A matrix `A` is decomposed into matrixes `Q` and `R` by applying the so-called Gram–Schmidt process:

![gs](https://wikimedia.org/api/rest_v1/media/math/render/svg/5c5078bb4f0bcbe9e2d0367e384593c31742f62f)

Note that each `u_i` and each `e_i` are column vectors. We can now write `Q` and `R` as:

![q](https://wikimedia.org/api/rest_v1/media/math/render/svg/ad022f6074ca3ab702e187026f090513a7ef2fbb)

![r](https://wikimedia.org/api/rest_v1/media/math/render/svg/c80c5ba160cebaaeb41a8c1c43cef2a07e909e6d)

Write a Python function `qr` that, given a matrix `A`, computes its `QR` decomposition and returns the two matrixes `Q` and `R`. You can check the correctness of your results by using function [numpy.linalg.qr](https://numpy.org/doc/stable/reference/generated/numpy.linalg.qr.html) and by comparing `A` with `Q @ R`.

Note: the `QR` decomposition is not unique up to the sign of the elements. In fact, since `Q` and `R` must be multiplied together to obtain `A`, it is possible to change the signs of the corresponding elements in `Q` and `R` while still satisfying `A = QR`. Thus, when comparing your results with the ones computed by numpy, you should focus on the abosulte values of your matrixes: `|Qcomputed| = |Qnumpy|` and `Rcomputed| = |Rnumpy|`. Remember to compare the matrixes using a tolerance.

## Template

```py
import numpy as np
def print_matrix(A):
  for row in range(len(A)):
    print(' ', ' '.join(f'{e:> #8.3f}' for e in A[row]))

if __name__ == '__main__':
  import sys
  n = int(input())
  arr = [int(x) for x in input().split()]
  A = np.array(arr, dtype=np.float32).reshape(n, n)

  Q, R = qr(A)
  Aback = Q @ R
  Qe, Re = np.linalg.qr(A)

  print(np.allclose(np.fabs(Q), np.fabs(Qe), atol=1e-4, rtol=1e-4))
  print(np.allclose(np.fabs(R), np.fabs(Re), atol=1e-4, rtol=1e-4))
  print(np.allclose(Q @ R, A, atol=1e-4, rtol=1e-4))
```

## Testcases

### 1

IN:
```
2
3 -5 -10 1
```

OUT:
```
True
True
True
```

### 2

IN:
```
4
2 -6 -7 -8 0 1 -3 3 2 7 1 3 -8 5 -1 0
```

OUT:
```
True
True
True
```

### 3

IN:
```
3
7 4 -7 -8 1 3 1 6 6
```

OUT:
```
True
True
True
```

### 4

IN:
```
4
5 -3 8 5 5 9 -3 -8 -2 -6 9 5 -10 5 1 -7
```

OUT:
```
True
True
True
```

### 5

IN:
```
5
1 -1 3 -9 6 -5 4 -6 -8 -7 -4 3 6 -3 -2 3 -9 3 5 -2 4 -10 -5 -7 -2
```

OUT:
```
True
True
True
```

### 6

IN:
```
8
6 3 -7 1 2 -10 -4 -2 9 7 9 -6 7 -8 -5 -1 4 -1 -1 4 5 5 6 4 2 3 4 9 3 2 -7 4 0 3 -4 7 7 6 -2 -3 -6 1 -1 0 -3 -8 -7 -10 -4 2 3 -7 1 -4 9 1 3 -1 -5 1 3 5 -6 5
```

OUT:
```
True
True
True
```

## Solution

```py
import math

def norm(v):
  return math.sqrt(np.square(np.fabs(v)).sum())

def vector_norm(u):
  return u / norm(u)

def projection(base, v):
  return ((v @ base) / (base @ base)) * base

def gs_process(A):
  n = A.shape[1]
  U = np.zeros_like(A)
  E = np.zeros_like(A)
  U[:, 0] = A[:, 0]
  E[:, 0] = vector_norm(U[:, 0])
  for k in range(1, n):
    U[:, k] = A[:, k].copy()
    for j in range(k):
      U[:, k] = U[:, k] - projection(U[:, j], A[:, k])
    E[:, k] = vector_norm(U[:, k])
  return U, E

def qr(A):
  U, E = gs_process(A)
  Q = E
  R = np.zeros_like(A)
  for row in range(A.shape[0]):
    for col in range(A.shape[1]):
      if col >= row:
        R[row, col] = A[:, col] @ E[:, row]
  return Q, R
```
