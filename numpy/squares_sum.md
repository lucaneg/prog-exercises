# Somma dei quadrati dei primi n numeri naturali

Scrivere una funzione `square_sum(n)` che dato un numero `n` ritorna la somma dei quadrati dei primi `n` numeri naturali (`n` incluso). La funzione non deve usare cicli, ma deve calcolare i quadrati usando la vettorizzazione di numpy per poi sommarli.

## Template

```py
if __name__ == '__main__':
    n = int(input())
    print(square_sum(n))
```

## Testcases

### 1

IN:
```
1
```

OUT:
```
1
```

### 2

IN:
```
2
```

OUT:
```
5
```

### 3

IN:
```
5
```

OUT:
```
55
```

### 4

IN:
```
10
```

OUT:
```
385
```

### 5

IN:
```
500
```

OUT:
```
41791750
```

## Solution

```py
import numpy as np

def square_sum(n):
  numbers = np.arange(1, n+1)
  numbers = numbers**2
  return numbers.sum()
```
