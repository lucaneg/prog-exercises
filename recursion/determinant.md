# Matrix determinant

Convert the following worklist algorithm for computing the determinant of a matrix, that we used to implement Gauss elimination, to its recursive version:

```py
def determinant_wl(A):
  stack = [(1, A)]
  det = 0
  while stack:
    factor, matrix = stack.pop()
    if matrix.size == 1:
      det += factor * matrix[0, 0]
    elif matrix.shape == (2, 2):
      det += factor * (matrix[0, 0] * matrix[1, 1] - matrix[0, 1] * matrix[1, 0])
    else:
      for col in range(matrix.shape[1]):
        e = matrix[0, col]
        coeff = 1 if col % 2 == 0 else -1
        cols = [c for c in range(matrix.shape[1]) if c != col]
        sub_matrix = matrix[1:, cols]
        stack.append((factor * e * coeff, sub_matrix))
  return det
```

The recursive function should be called `determinant(matrix)` and given a matrix it should return its determinant.

## Template

```py
if __name__ == '__main__':
    import random
    random.seed(int(input()))
    import numpy as np
    n = random.randint(2, 5)
    rng = np.random.default_rng()
    mat = rng.integers(-10, 10, size=(n,n)).astype(np.float32)
    print(determinant(mat) == np.linalg.det(mat))
```

## Testcases

### 1

IN:
```
1
```

OUT:
```
179.0
```

### 2

IN:
```
2
```

OUT:
```
70.0
```

### 3

IN:
```
3
```

OUT:
```
1208.0
```

### 4

IN:
```
4
```

OUT:
```
130.0
```

## Solution

```py
def determinant(A):
    if A.size == 1:
        return A[0, 0]
    elif A.shape == (2, 2):
        return (A[0, 0] * A[1, 1] - A[0, 1] * A[1, 0])
    else:
        det = 0
        for col in range(A.shape[1]):
            e = A[0, col]
            coeff = 1 if col % 2 == 0 else -1
            cols = [c for c in range(A.shape[1]) if c != col]
            sub_matrix = A[1:, cols]
            det += coeff * e * determinant(sub_matrix)
    return det
```
