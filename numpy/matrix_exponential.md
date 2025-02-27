# Esponenziale di una matrice

Data una matrice quadrata `X`, il suo esponenziale è dato dalla formula:

![img](https://wikimedia.org/api/rest_v1/media/math/render/svg/c19bb8d90896883a113259bfaddf3f6bdcc89047)

dove `X^k` **non corrisponde a X&ast;&ast;k, ma a X @ X @ ... k volte.**

Scrivere una funzione `mat_exp(matrix, n_terms)` che, data una matrice quadrata `matrix` ritorna un'approssimazione del suo esponenziale. L'approssimazione viene calcolata sommando le prime `n_terms` potenze di `matrix` (ricordandosi che `matrix ** 0` è sempre uguale alla matrice identità, che è possibile creare con [numpy.eye](https://numpy.org/devdocs/reference/generated/numpy.eye.html)). E' consentito l'uso di un solo ciclo for per iterare su `n_terms`.

## Template

```py
if __name__ == '__main__':
    def print_matrix(A):
        for row in range(A.shape[0]):
            print(' ', ' '.join(f'{e:> #8.5f}' for e in A[row]))

    import numpy as np
    seed = int(input())
    n_terms = int(input())
    import random
    random.seed(seed)
    n = random.randint(2, 5)
    rng = np.random.default_rng(seed=seed)
    A = rng.integers(-10, 10, size=(n,n)).astype(np.float64)
    print_matrix(mat_exp(A, 100))
```

## Testcases

### 1

IN:
```
5
100
```

OUT:
```
   1.56440  3.54867  2.33224 -2.09601
   1.60241  3.91885 -0.27885 -0.65542
  -0.64161  2.78822  1.02937 -1.34276
  -0.11418 -2.28144  0.25505  0.39990
```

### 2

IN:
```
8
100
```

OUT:
```
   5.85408 -1.71772 -2.74477
   5.67892 -0.65967 -1.33328
  -0.29445  3.48066  4.68287
```

### 3

IN:
```
10
100
```

OUT:
```
  -1.02319 -0.91455
   0.50808  0.09459
```

### 4

IN:
```
9
100
```

OUT:
```
   400808.39654  366816.58418  454983.62087  258220.21936 -38339.46193
   978274.06812  923873.94320  1166198.39777  601138.31910 -49837.47260
   915109.25326  847454.40424  1058208.56268  579412.63979 -72293.14717
  -2239120.94826 -2131650.19355 -2702481.54298 -1358544.40112  87973.78672
   2499200.05688  2377088.78454  3012174.59425  1518541.34659 -101495.68324
```

### 5

IN:
```
11
100
```

OUT:
```
   3922.68236 -10463.47090  1959.66002 -7101.25984  5839.05739
  -4771.30451  12727.05993 -2385.72729  8631.38606 -7108.29595
   4736.04106 -12633.46878  2364.04383 -8580.10327  7043.77982
  -10675.52577  28476.12557 -5336.55186  19317.05603 -15900.05639
   311.05143 -829.35193  160.27378 -548.90794  476.95114
```

## Solution

```py
import math
def mat_exp(matrix, n_terms):
  result = np.eye(matrix.shape[0])
  term = np.eye(matrix.shape[0])
  for k in range(1, n_terms):
    term = term @ matrix
    result = result + (term / math.factorial(k))
  return result
```
