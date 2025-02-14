# Numeri triangolari

Un numero intero `n` si dice *poligonale* se è possibile disporre `n` oggetti su una superfice creando un poligono regolare:

![square](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Polygonal_Number_4.gif/500px-Polygonal_Number_4.gif)

Un numero poligonale si dice *triangolare* se è possibile disporlo per creare un triangolo equilatero:

![triangle](https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/Polygonal_Number_3.gif/500px-Polygonal_Number_3.gif)

L'*n*-esimo numero triangolare è calcolabile tramite la seguente formula:

![formula](https://wikimedia.org/api/rest_v1/media/math/render/svg/c38b21816ac2c7ffc218389b16b4de8ebf8b927d)

dando origine alla sequenza:

| Indice | 0 | 1 | 2 | 3 | 4  | 5  | ... |
| ------ | - | - | - | - | -- | -- | --- |
| Numero | 0 | 1 | 3 | 6 | 10 | 15 | ... |

Scrivere una funzione `triangular_numbers(n)` che, dato un numero `n`, ritorna la sequenza di numeri triangolari con indici da `0` a `n` **incluso**. La funzione non dovrà contenere cicli, ma dovrà usare NumPy per (i) generare gli indici necessari, e (ii) trasformare gli indici nei numeri triangolari corrispondenti utilizzando la vettorizzazione. La funzione dovrà ritornare **solo i numeri triangolari**, senza i loro indici, come un array di NumPy.

## Template

```py
import numpy as np
if __name__ == '__main__':
  print(triangular_numbers(int(input())))
```

## Testcases

### 1

IN:
```
0
```

OUT:
```
[0]
```

### 2

IN:
```
1
```

OUT:
```
[0 1]
```

### 3

IN:
```
2
```

OUT:
```
[0 1 3]
```

### 4

IN:
```
5
```

OUT:
```
[ 0  1  3  6 10 15]
```

### 5

IN:
```
30
```

OUT:
```
[  0   1   3   6  10  15  21  28  36  45  55  66  78  91 105 120 136 153
 171 190 210 231 253 276 300 325 351 378 406 435 465]
```

## Solution

```py
def triangular_numbers(n):
  indices = np.arange(0, n+1)
  triangular_seq = (indices * (indices + 1)) // 2
  return triangular_seq
```
