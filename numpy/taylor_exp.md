# Serie di Taylor per e^x

La funzione `e**x` può essere decomposta e approssimata nella seguente serie di Taylor:

![img](https://wikimedia.org/api/rest_v1/media/math/render/svg/229ab1dbc98242ae3ec8b35e1eb882561f3158bb)

ovvero:

![img](https://wikimedia.org/api/rest_v1/media/math/render/svg/84555207246856cf319c6d825007850a8f1c62ff)

Scrivere una funzione `taylor(n, x)` che, dato un numero intero `n` e un valore `x`, calcola i primi `n` termini della serie di Taylor per `e**x` senza usare cicli in un array di numpy, per poi ritornarne la somma (ovvero l'approssimazione di `e**x`).

## Template

```py
if __name__ == '__main__':
    n = int(input())
    x = float(input())
    print(taylor(n, x))
```

## Testcases

### 1

IN:
```
50
1
```

OUT:
```
2.718281828459045
```

### 2

IN:
```
25
0.38
```

OUT:
```
1.4622845894342242
```

### 3

IN:
```
100
1.7
```

OUT:
```
5.4739473917272
```

### 4

IN:
```
100
-2.6
```

OUT:
```
0.07427357821433422
```

## Solution

```py
import numpy as np
def taylor(n, x):
    terms = np.arange(n)
    terms = x ** terms
    factorial = 1
    for i in range(2, n):
        factorial = factorial * i
        terms[i] = terms[i] / factorial
    return terms.sum()
```
