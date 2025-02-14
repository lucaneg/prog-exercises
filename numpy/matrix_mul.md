# Moltiplicazione tra matrici

Scrivere una funzione `mul(A, B)` che, date due matrici quadrate di dimensioni sconosciute ritorna la loro moltiplicazione **senza usare l'operatore @**.

## Template

```py
if __name__ == '__main__':
    import numpy as np
    n = int(input())
    r = int(input())
    m = int(input())
    seed = int(input())
    rng = np.random.default_rng(seed)
    A = rng.integers(-10, 10, size=(n,r))
    B = rng.integers(-10, 10, size=(r,m))
    print(mul(A, B))
```

## Testcases

### 1

IN:
```
2
5
4
7
```

OUT:
```
[[ -85.   28.   93. -117.]
 [  17.  130.  -76.   49.]]
```

### 2

IN:
```
4
4
4
7
```

OUT:
```
[[ -47.   17.  -38.  -17.]
 [  52.  -45.   20.  -44.]
 [  21.   -4.   50.   46.]
 [-134.   85.  -27.   50.]]
```

### 3

IN:
```
6
5
2
7
```

OUT:
```
[[ 50.  64.]
 [-51. -25.]
 [  7.  69.]
 [  8. -66.]
 [-54. -24.]
 [-21.  64.]]
```

## Solution

```py
def mul(A, B):
    result = np.zeros((A.shape[0], B.shape[1]))
    for row in range(result.shape[0]):
        for col in range(result.shape[1]):
            for i in range(A.shape[1]):
                result[row, col] = result[row, col] + A[row, i] * B[i, col]
    return result
```
